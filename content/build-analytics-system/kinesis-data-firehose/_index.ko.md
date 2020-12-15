---
title: "Kinesis Data Firehose"
weight: 32
pre: "<b>3-2. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.svg)

## 데이터를 S3에 저장하기 위한 Kinesis Data Firehose 생성하기

{{% notice info %}}
Kinesis Data Firehose를 이용해서 실시간으로 데이터를 S3, Redshift, ElasticSearch 등의 목적지에 수집할 수 있습니다.
{{% /notice %}}

AWS Management Console에서 Kinesis 서비스를 선택합니다.

1. Get Started 버튼을 클릭합니다.
2. Deliver streaming data with Kinesis Firehose delivery streams 메뉴의 **\[Create delivery stream\]** 을 클릭하여
새로운 Firehose 전송 스트림 생성을 시작합니다.
3. (Step 1: Name and source) Delivery stream name에 원하는 이름(예: `retail-trans`)를 입력합니다.
4. **Choose a source** 에서 `Kinesis Data Stream` 를 선택하고, 앞서 생성한 Kinesis Data Stream(예: `retail-trans`)을 선택 한 후,
**Next**를 클릭합니다.
5. (Step 2: Process records) **Transform source records with AWS Lambda / Convert record format** 은 
둘다 default 옵션 `Disabled`를 선택하고 **Next**를 클릭합니다.
6. (Step 3: Choose a destination) Destination은 Amazon S3를 선택하고, `Create new` 를 클릭해서 S3 bucket을 생성합니다.
S3 bucket 이름은 이번 실습에서는 `aws-analytics-immersion-day-xxxxxxxx` 형식으로 `xxxxxxxx` 는 bucket 이름이 겹치지 않도록 임의의 숫자나
문자를 입력 합니다.

    S3 prefix를 입력합니다. 예를 들어서 다음과 같이 입력 합니다.
    
    ```buildoutcfg
    json-data/year=!{timestamp:yyyy}/month=!{timestamp:MM}/day=!{timestamp:dd}/hour=!{timestamp:HH}/
    ```

    S3 error prefix를 입력합니다. 예를 들어서 다음과 같이 입력 합니다.
    ```buildoutcfg
    error-json/year=!{timestamp:yyyy}/month=!{timestamp:MM}/day=!{timestamp:dd}/hour=!{timestamp:HH}/!{firehose:error-output-type}
    ```
    S3 prefix와 3 error prefix 입력을 완료한 후에, Next를 클릭합니다. 
    (참고: [https://docs.aws.amazon.com/firehose/latest/dev/s3-prefixes.html](https://docs.aws.amazon.com/firehose/latest/dev/s3-prefixes.html))
7. (Step 4: Configure settings) S3 buffer conditions에서 Buffer size는 `1MB`, Buffer interval은 `60` seconds로 설정합니다.
8. 아래 IAM role에서 **\[Create new, or Choose\]** 버튼을 클릭합니다.
9. 새로 열린 탭에서 필요한 정책이 포함된 IAM 역할 `firehose_delivery_role`을 자동으로 생성합니다. Allow 버튼을 클릭하여 진행합니다.
![kfh_create_new_iam_role](/analytics-on-aws/images/kfh_create_new_iam_role.png)
10. 새롭게 생성된 역할이 추가된 것을 확인한 뒤 Next 버튼을 클릭합니다.
11. (Step 5: Review) Review에서 입력한 정보를 확인한 뒤 틀린 부분이 없다면, **\[Create delivery stream\]** 버튼을 클릭하여 Firehose 생성을 완료합니다.


