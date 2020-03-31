---
title: "AWS Lambda"
weight: 38
pre: "<b>3-8. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## AWS Lambda Function을 이용해서 실시간 데이터를 ElasticSearch에 수집하기

Lambda function을 이용해서 Amazon ES에 데이터를 실시간으로 색인할 수 있습니다.
이번 실습에서는 AWS Lambda 콘솔을 사용하여 Lambda 함수를 생성합니다.

### Lambda 함수에서 사용할 공통 라이브러를 Layers에 추가하려면,
1. **AWS Lambda 콘솔** 을 엽니다.
2. **Layers** 메뉴에 들어가서 **\[Create layer\]** 을 선택합니다.
3. Name에 `es-lib` 를 입력합니다.
4. `Upload a file from Amazon S3` 를 선택하고, 라이브러리 코드가 저장된 s3 link url 또는 압축한 라이브러리 코드 파일을 입력합니다.
이번 실습에서는 `resources/es-lib.zip` 파일을 사용합니다. `es-lib.zip` 생성 방법은 
[AWS Lambda Layer에 등록할 Python 패키지 생성 예제](/ko/reference/) 를 참고하세요.
5. `Compatible runtimes` 에서 `Python 3.8` 을 선택합니다.
![aws-lambda-create-layer](/analytics-on-aws/images/aws-lambda-create-layer.png)

### Lambda 함수를 생성하려면,
1. **AWS Lambda 콘솔** 을 엽니다.
2. **\[Create a function\]** 을 선택합니다.
3. Function name(함수 이름)에 `UpsertToES` 을 입력합니다.
4. Runtime 에서 `Python 3.8` 을 선택합니다.
5. **\[Create a function\]** 을 선택합니다.
![aws-lambda-create-function](/analytics-on-aws/images/aws-lambda-create-function.png)
6. Designer(디자이너) 에서 layers를 선택합니다. Layers에서 Add a layer를 선택합니다.
7. Layer selection에서 `Select from list of runtime compatible layers`을 클릭하고, 
Compatiable layers에서 Name과 Version으로 앞서 생성한 layer의 Name과 Version을 선택합니다.
![aws-lambda-add-layer-to-function](/analytics-on-aws/images/aws-lambda-add-layer-to-function.png)
Layer의 runtime 버전과 Lambda function의 runtime이 버전이 다른 경우, Layer selection의 Compatiable layers에 필요한 layer가
목록에 보이지 않을 수 있습니다. 
이러한 경우, Layer selection에서 `Provide a layer version ARN`을 클릭하고,
layer의 arn을 직접 입력하면 됩니다.
![aws-lambda-add-layer-to-function-layer-version-arn](/analytics-on-aws/images/aws-lambda-add-layer-to-function-layer-version-arn.png)
8. **\[Add\]** 클릭합니다.
9. Designer(디자이너) 에서 `UpsertToES` 을 선택하여 함수의 코드 및 구성으로 돌아갑니다.
10. Function code의 코드 편집기에 `upsert_to_es.py` 파일의 코드를 복사해서 붙여넣습니다.
11. Environment variables 에서 **\[Edit\]** 를 클릭합니다.
12. **\[Add environment variables\]** 를 클릭해서 아래 4개의 Environment variables을 등록합니다.
    ```shell script
    ES_HOST=<elasticsearch service domain>
    REQUIRED_FIELDS=<primary key로 사용될 column 목록>
    REGION_NAME=<region-name>
    DATE_TYPE_FIELDS=<column 중, date 또는 timestamp 데이터 타입의 column>
    ```
    예를 들어, 다음과 같이 Environment variables을 설정합니다.
    ```buildoutcfg
    ES_HOST=vpc-retail-xkl5jpog76d5abzhg4kyfilymq.us-west-1.es.amazonaws.com
    REQUIRED_FIELDS=Invoice,StockCode,Customer_ID
    REGION_NAME=us-west-2
    DATE_TYPE_FIELDS=InvoiceDate
    ```
13. **\[Save\]** 선택합니다.
14. lambda 함수를 VPC 내에서 실행 하고, Kinesis Data Streams에서 데이터를 읽기 위해서,
lamba 함수 실행에 필요한 Execution role에 필요한 IAM Policy를 추가햐야 합니다.
IAM Role 수정을 위해서 `View the UpsertToES-role-XXXXXXXX role on the IAM console.` 을 클릭 합니다.
![aws-lambda-execution-iam-role](/analytics-on-aws/images/aws-lambda-execution-iam-role.png)
15. IAM Role의 **\[Permissions\]** 탭에서 **\[Attach policies\]** 버튼을 클릭 후, 
**AWSLambdaVPCAccessExecutionRole**, **AmazonKinesisReadOnlyAccess** 를 차례로 추가 합니다.
![aws-lambda-iam-role-policies](/analytics-on-aws/images/aws-lambda-iam-role-policies.png)
16. VPC 항목에서 **\[Edit\]** 버튼을 클릭해서 Edit VPC 화면으로 이동 한다. VPC connection 에서 `Custom VPC` 를 선택합니다.
Elasticsearch service의 도메인을 생성한 VPC와 subnets을 선택하고, Elasticsearch service 도메인에 접근이 허용된
security groups을 선택합니다.
17. Basic settings에서 **\[Edit\]** 선택합니다. Memory와 Timeout을 알맞게 조정합니다. 이 실습에서는 Timout을 `5 min` 으로 설정합니다.
18. 다시 Designer 탭으로 돌아가서 **\[Add trigger\]** 를 선택합니다.
19. **Trigger configuration** 의 `Select a trigger` 에서 **Kinesis** 를 선택 합니다.
20. Kinesis stream 에서 앞서 생성한 Kinesis Data Stream(`retail-trans`)을 선택합니다.
21. **\[Add\]** 를 선택합니다.
![aws-lambda-kinesis](/analytics-on-aws/images/aws-lambda-kinesis.png)

