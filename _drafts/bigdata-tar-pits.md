---
title: "Big Data Tar Pits"
layout: post
date: 2024-01-09
tags: big data, architecture
---

# Outline

- What is Big Data
- Tar Pits
    - [X] Data Partitioning
    - [ ] Reprocessing Plan
    - [ ] Pre-Compute Aggregates
    - [ ] At Lease Once vs. At Most Once vs. Exactly Once
    - [X] Time is an Illusion

# What is Big Data

My personal definition of Big Data is:

> Any Data Processing that requires special considerations to overcome time and/or space limitations.
> _Yours Truly_

Which lines up pretty closely with Wikipedia's definition of Big Data:

> Big data primarily refers to data sets that are too large or complex to be dealt with by traditional data-processing application software.
> _[Wikipedia](https://en.wikipedia.org/wiki/Big_data)_

In essence "Big Data" is an ambiguous term that is generally understood to mean any data that can't processed using "traditional", for some definition of traditional, data processing mechanisms. The need for special data processing usually stems from either the speed which the data is changing, call the data velocity; Or the size of the data, called the volume.

Managing big data usually means designing a system that will allow for distributing the data, and the processing of the data, across multiple compute and storage nodes in a cluster. Even if the exact mechanics of distributing the data and computation is abstracted behind a framework like [Hadoop](https://hadoop.apache.org/), [Spark](https://spark.apache.org/), or [Flink](https://flink.apache.org/) the distributed nature of the system needs to be taken into account. 

# Tar Pits

To help discuss our tar pits we are going to make up an example use case. Our use case is going to be a game that sends a players current location on the game map to our system. Our system needs to collect the events, store them and make them available for ad-hoc query analysis. In addition our system needs to:

- Maintain a real-time map of current player locations.
- Maintain a per-player timeline of the player's movements around the map.

## Time is an Illusion

When processing data in a big data system, you are most likely processing multiple data items in parallel across a cluster of machines. The distributed and parallel nature of the processing means that you lose most guarantees over message ordering, especially global ordering. 

In our game, multiple users are sending us messages with updates to their location. User A might live closer to our data center than user B. It is easy to imagine how an message from user B might arrive after a message from user A, even though the event occurred and was sent before user A's message. Even further once the messages are received the are put on a queue (e.g. a Kafka queue) and as nodes on the cluster become free they will pickup the next available message form the queue. A node might pick up a message from user A, and another node pickup the message from user B. The second node might finish processing message B and send the result down to the next step in the pipeline before the first node is done processing message A. The point is even if you could receive messages in order, processing them in parallel means you lose any guarantees that the messages will finish processing in the same order.

A seemingly easy solution to this whole mess is to use timestamps to order events. Basically our game will attach a timestamp to each message before the message is sent. The issue here is that you can't rely on client machines to have accurate timestamps. You can't even rely on machines you control to have accurate and synchronized timestamps. Timestamps on a single machine can jitter giving subsequent messages timestamps that occur "before" previous messages.

Relying on timestamps to order events is a common tar pit I see big data solutions fall into. The issue with using timestamps is that they work 90% of the time and might honestly be good enough for your use case. However a much more reliable and future-proof solution is to utilize and explicit ordering counter, like a sequence number. You cannot achieve global ordering, but you can achieve strong local ordering by using an event sequence number.

Going back to our example, there is no way to achieve exact ordering of messages across all player, but we don't need global ordering either. What we need is ordering of events per-player. Like we said we can't use timestamps to order events even for a single player session. What we can do though is use a sequence number where each time the game sends a message for a player it increments that player's event sequence number.

When developing the map of the current player locations, whenever we receive a new location from a player we can check the sequence number to see if the "new" location is really newer than the last location we have for that player or not. Keep in mind that this check-and-update operation needs to be an [atomic operation](https://stackoverflow.com/questions/52196678/what-are-atomic-operations-for-newbies). Most distributed key-value stores, like HBase and DynamoDB, and relational databases, like Postgres, provide some sort of atomic check-and-update operation.

Along the same lines each time we receive an event from a player we can order the events by sequence number to make sure we are displaying the timeline of movements in the correct order. 

------------------

## Data Partitioning

By definition Big Data sets are too large to process as one single sum. In all but the most extreme cases you will want to process only a subset of the data and partitioning is how the data is sliced up into subsets. Partitioning in the big data world usually comes in two flavors: Partitioning the data for processing and partitioning the data for storage and retrieval, i.e. querying and analysis.

### Partitioning Data For Processing

Ideally partitioning the data during processing should allow for the uniform distribution of the data across the available computation nodes. In other words we want each of the nodes in the cluster to process a similar amount of data, for new data to be available for node to process as soon as it is done processing the current data item, and for a node to be available to process a data item as soon as the data is created. At the same time there might be ordering constraints that need to be taken into account or a need to group similar items during processing.

Partitioning the data during processing also provide some partial ordering guarantees. A partitioning scheme that guarantees that all the events for a given player will be processed by the same node means that the node can inspect the state of the current player knowing that no other node will also be inspecting and updating the same player's state. In our example we would want a data processing partitioning scheme that will guarantees that all messages from a given player are processed by the same node, so that the processing node can inspect the current "last known" location and possibly update it without other nodes also updating the same player's current location causing race conditions. We might not want the entire data processing pipeline to utilize the same partitioning scheme, but we do want the portion that checks and updates the current state of the player to use such a scheme. 

If we do follow such a partitioning scheme when updating a users latest location, we won't need an atomic check-and-update operation any more since we know that only one node will be updating a given players location. In other words we might be updating the latest location of multiple players in parallel, but for any given player, because of how we partitioned the data, we know that only one node will be updating that players latest location. The downside is that we might have a sub-optimal partitioning scheme from a data processing perspective. If you have 100 nodes, but only have 70 players currently playing the game, our partitioning scheme means that at-most 70 nodes will be used for processing the data from those players and that at least 30 nodes will be sitting idle without any data to process.

### Partitioning Data for Storage 

Partitioning the data for storage and retrieval is a little more complex because the "ideal" partitioning scheme doesn't exist. Instead how the data is partitioned will highly depend on how the data is expected to be accessed. To illustrate that let assume you want to two types for analysis on your accumulated player data:

The first type of analysis is to build up reports and aggregates on how many players performed certain actions or reach certain milestones over a given time line. For example how many players beat level 10 this month compared to last month. In SQL that would look something like this:

*Action Frequency*

```sql
SELECT 
	actions.type, sum(1) as cnt 
FROM mydb.actions 
WHERE 
	actions.timestamp > MONTH_START and actions.timestamp < MONTH_END 
	and actions.type in (LIST OF EVENT TYPES I CARE ABOUT)
GROUP BY actions.type;
```

#### Case 1: No Partitioning

No partitioning is not a practical approach because if your data is all in just one big blob, that blob will eventually become too big to read and process. At the very least data is usually partitioned by date, which in this case would mean the queries above could limit their search to just the data in the date range they need but also means the queries will have to parse through _all_ the data in that date range.

For example, the "Action Frequency" will have to parse through the all actions of each player, instead of being able to focus on specifically the actions listed in the query.  

#### Case 2: Partitioning By Action Type - Player - Date

With this partitioning schema the "Action Frequency" query will first find the partitions for only the action types in the query, and then for each player sub-partition will only read the data for the dates within the query's range. This partitioning schema allows the query engine to hone in and read only the data required to execute the query. 

At the same time this partitioning scheme is designed to fit the queries criteria. If a different query is used, for example to find the location where each player was last seen, this partition schema would be of little help and the query engine would still have to read all the data for all the actions for all the players within the given date range. Understanding the types of queries and analysis you expect to be performed on the data, especially the attributes most likely to be used as filters, will help you design a partitioning schema to fit those queries.

------------------------------------

# Have a Reprocessing Plan

When working with a distributed system, the question isn't _if_ something will fail, it is _when_ will it fail.
