---
title: "Big Data Tar Pits"
layout: post
date: 2024-01-09
tags: big data, architecture
---

# Outline

- What is Big Data
- Tar Pits
    - Data Partitioning
    - Reprocessing Plan
    - Pre-Compute Aggregates
    - At Lease Once vs. At Most Once vs. Exactly Once
    - Time is an Illusion

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
- Maintain a per-player heat map of the player's movements around the map.

## Time is an Illusion

When processing data in a big data system, you are processing multiple data items in parallel across a cluster of machines. The distributed and parallel nature of the processing means that you lose most guarantees over message ordering, especially global ordering. 

In our game, multiple users are sending us messages with updates to their location. User A might live closer to our data center than user B. It is easy to imagine how an message from user B might arrive after a message from user A, even though the event occurred and was sent before user A's message. Even further once the messages are received the are put on a queue (e.g. a Kafka queue) and as nodes on the cluster become free they will pickup the next available message form the queue. A node might pick up a message from user A, and another node pickup the message from user B. The second node might finish processing message B and send the result down to the next step in the pipeline before the first node is done processing message A. The point is even if you could receive messages in order, processing them in parallel means you lose any guarantees that the messages will finish processing in the same order.

A seemingly easy solution to this whole mess is to use timestamps to order events. Basically our game will attach a timestamp to each message before the message is sent. The issue here is that you can't rely on client machines to have accurate timestamps. You can't even rely on machines you control to have accurate and synchronized timestamps. Timestamps on a single machine can jitter given subsequent messages timestamps that occur "before" previous messages. TODO: Add reference links.

Relying on timestamps to order events is a common tar pit I see big data solutions fall into. The issue with using timestamps is that they work 90% of the time and might honestly be good enough for your use case. However a much more reliable and future-proof solution is to utilize and explicit ordering counter, like a sequence number. You cannot achieve global ordering, but you can achieve strong local ordering by using an event sequence number.

Going back to our example, there is no way to achieve exact ordering of messages across all users, but we don't need global ordering either. What we need is ordering of events per-player. Like we said we can't use timestamps to order events even for a single player session. What we can do though is use a sequence number where each time the game sends a message for a player it increments that player's event sequence number. When ordering events for a given player we can use the sequence number to determine the order of events.

## Data Partitioning

By definition Big Data sets are too large to process as one single sum. In all but the most extreme cases you will want to process only a subset of the data and partitioning is how the data is sliced up into given subsets. Partitioning in the big data world usually comes in two flavors: Partitioning the data for processing and partitioning the data for storage and retrieval. 

Ideally partitioning the data during processing should allow for the uniform distribution of the data across the available computation nodes. In other words we want each of the nodes in the cluster to process a similar amount of data, for new data to be available for node to process as soon as it is done processing the current data item, and for a node to be available to process data item as soon as the data is created. At the same time there might be ordering constraints that need to be taken into account or a need to group similar items during processing.

Partitioning the data for storage and retrieval is a little more complex because the "ideal" partitioning scheme doesn't exist. Instead how the data is partitioned will highly depend on how the data is expected to be accessed.
