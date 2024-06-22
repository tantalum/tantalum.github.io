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

_Yours Truly_

My personal definition lines up pretty closely with Wikipedia's definition of Big Data:

> Big data primarily refers to data sets that are too large or complex to be dealt with by traditional data-processing application software.

_[Wikipedia](https://en.wikipedia.org/wiki/Big_data)_

In essence "Big Data" is a pretty ambiguous term that is generally understood to mean any data that can't processed using "traditional", for some definition of traditional, data processing mechanisms. The need for special data processing usually stems from either the speed which the data is changing, call the data velocity; Or the size of the data, technically called the volume.

Managing big data usually means designing a system that will allow for distributing the data, and the processing of the data, across multiple compute and storage nodes in a cluster. Even if the exact mechanics of distributing the data and computation is abstracted behind a framework like [Hadoop](https://hadoop.apache.org/), [Spark](https://spark.apache.org/), or [Flink](https://flink.apache.org/) the distributed nature of the system needs to be taken into account. 

# Tar Pits

## Data Partitioning

By definition Big Data sets are too large to process as one single sum. In all but the most extreme cases you will want to process only a subset of the data and partitioning is how the data is sliced up into given subsets. Partitioning in the big data world usually comes in two flavors: Partitioning the data for processing and partitioning the data for storage and retrieval. 

Ideally partitioning the data during processing should allow for the uniform distribution of the data across the available computation nodes. In other words we want each of the nodes in the cluster to process a similar amount of data, for new data to be available for node to process as soon as it is done processing the current data item, and for a node to be available to process data item as soon as the data is created. 
