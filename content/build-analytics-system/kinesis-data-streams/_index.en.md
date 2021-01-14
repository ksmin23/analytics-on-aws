---
title: "Kinesis Data Streams"
weight: 31
pre: "<b>3-1. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.svg)

## Create Kinesis Data Streams to receive input data

Select Kinesis services the AWS Management Console.
1. Click [this link](https://console.aws.amazon.com/kinesis/home#/streams/create) to create Kinesis Data Stream.
2. Enter the desired name for **Kinesis stream name** (e.g. `retail-trans`).
3. In **Number of shards**, enter the desired number of shards (e.g. `1`).
4. Click the **\[Create data stream\]** button and wait for the status of the created kinesis stream to become active.
