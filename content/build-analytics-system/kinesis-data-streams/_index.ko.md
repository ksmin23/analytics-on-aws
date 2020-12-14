---
title: "Kinesis Data Streams"
weight: 31
pre: "<b>3-1. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## 입력 데이터를 수신할 Kinesis Data Streams 생성하기

AWS Management Console에서 Kinesis 서비스를 선택합니다.
1. [이 링크](https://us-west-2.console.aws.amazon.com/kinesis/home?region=us-west-2#/streams/create)를 클릭하여 Kinsis Data Stream 생성을 시작합니다.
2. Kinesis stream name 에 원하는 이름(예: `retail-trans`)을 입력합니다.
3. Number of shards 에 원하는 shards 수(예: `1`)를 입력합니다.
4. **\[Create data stream\]** 버튼을 클릭 후, 생성된 kinesis stream의 status가 active가 될 때까지 기다립니다.

