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

