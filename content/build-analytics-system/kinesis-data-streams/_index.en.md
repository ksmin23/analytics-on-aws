---
title: "Kinesis Data Streams"
weight: 31
pre: "<b>3-1. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## Create Kinesis Data Streams to receive input data

Select Kinesis services the AWS Management Console.
1. Click [this link](https://us-west-2.console.aws.amazon.com/kinesis/home?region=us-west-2#/streams/create) to create Kinesis Data Stream.
3. Enter the desired name for **Kinesis stream name** (e.g. `retail-trans`).
4. In **Number of shards**, enter the desired number of shards (e.g. `1`).
5. Click the **\[Create data stream\]** button and wait for the status of the created kinesis stream to become active.
