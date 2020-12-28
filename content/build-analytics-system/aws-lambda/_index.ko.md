---
title: "AWS Lambda"
weight: 38
pre: "<b>3-8. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## AWS Lambda Function을 이용해서 실시간 데이터를 ElasticSearch에 수집하기

Lambda function을 이용해서 Amazon ES에 데이터를 실시간으로 색인할 수 있습니다.
이번 실습에서는 AWS Lambda 콘솔을 사용하여 Lambda 함수를 생성합니다.

### Lambda 함수에서 사용할 공통 라이브러리를 Layers에 추가하기
1. [이 링크](https://github.com/ksmin23/aws-analytics-immersion-day/raw/main/resources/es-lib.zip)를 클릭하여 파이썬 패키지 파일을 다운 받습니다. `es-lib.zip` 생성 방법은 
[AWS Lambda Layer에 등록할 Python 패키지 생성 예제](/ko/reference/) 를 참고하세요.
1. **[AWS Lambda 콘솔](https://console.aws.amazon.com/lambda/home)** 을 엽니다.
2. **Layers** 메뉴에 들어가서 **\[Create layer\]** 을 선택합니다.
3. Name에 `es-lib` 를 입력합니다.
4. **Upload a .zip file** 를 선택하고, **Upload** 버튼을 클릭하여 다운 받은 es-lib.zip 파일을 지정합니다.
5. `Compatible runtimes` 에서 `Python 3.8` 을 선택합니다.
![aws-lambda-create-layer](/analytics-on-aws/images/aws-lambda-create-layer.png)

### 이 Lambda Layer를 사용하는 Lambda 함수를 생성하기
1. **[AWS Lambda 콘솔](https://console.aws.amazon.com/lambda/home)** 을 엽니다.
2. **\[Create function\]** 을 선택합니다.
   * Function name(함수 이름)에 `UpsertToES` 을 입력합니다.
   * Runtime 에서 `Python 3.8` 을 선택합니다.

   ![aws-lambda-create-function](/analytics-on-aws/images/aws-lambda-create-function.png)

3. **\[Create function\]** 을 선택합니다.
4. Designer 패널에서 중앙부에 있는 **Layers**를 클릭합니다. Layers 패널이 하단에 로드되면, **Add a layer**를 선택합니다.
5. Custom layers 를 클릭하고 아래와 같이 앞서 생성한 es-lib 레이어를 선택합니다. 버전은 1로 선택합니다.
   ![](/analytics-on-aws/images/lambda-layer.png)

6. **\[Add\]** 클릭합니다.
7. Designer(디자이너) 에서 `UpsertToES` 을 선택하여 함수의 코드 및 구성으로 돌아갑니다.
8.  Function code의 코드 편집기에 [이 링크](https://raw.githubusercontent.com/ksmin23/aws-analytics-immersion-day/main/src/main/python/UpsertToES/upsert_to_es.py)의 코드를 복사해서 붙여넣은 뒤 Deploy 를 클릭합니다.
   ![](/analytics-on-aws/images/lambda-upsert-to-es.png)

9.  Environment variables 패널에서 **\[Edit\]** 를 클릭합니다.
10. **\[Add environment variables\]** 를 클릭해서 아래의 Environment variables을 등록합니다.
    ```shell script
    ES_HOST=<elasticsearch service domain>
    ES_INDEX=<elasticsearch index name>
    ES_TYPE=<elasticsearch type name>
    REQUIRED_FIELDS=<primary key로 사용될 column 목록>
    REGION_NAME=<region-name>
    DATE_TYPE_FIELDS=<column 중, date 또는 timestamp 데이터 타입의 column>
    ```
    예를 들어, 다음과 같이 Environment variables을 설정합니다.
    ```buildoutcfg
    ES_HOST=vpc-retail-xkl5jpog76d5abzhg4kyfilymq.us-west-1.es.amazonaws.com
    ES_INDEX=retail
    ES_TYPE=trans
    REQUIRED_FIELDS=Invoice,StockCode,Customer_ID
    REGION_NAME=us-west-2
    DATE_TYPE_FIELDS=InvoiceDate
    ```
    
11. **\[Save\]** 선택합니다. Environment Variables 를 정상적으로 입력하면 아래와 같이 확인할 수 있습니다.
    ![](/analytics-on-aws/images/lambda-upsert-env.png)
12. **Basic Setting** 패널에서 **[Edit]** 을 클릭합니다. 
    1.  lambda 함수를 VPC 내에서 실행 하고 Kinesis Data Streams에서 데이터를 읽기 위해서, lamba 함수 실행에 필요한 Execution role에 필요한 IAM Policy를 추가해야 합니다.
    IAM Role 수정을 위해서 최하단의 `View the UpsertToES-role-XXXXXXXX role on the IAM console.` 을 클릭 합니다.
    ![aws-lambda-execution-iam-role](/analytics-on-aws/images/lambda-upsert-role.png)
    1.  IAM Role의 **\[Permissions\]** 탭에서 **\[Attach policies\]** 버튼을 클릭 후, 
    **AWSLambdaVPCAccessExecutionRole**, **AmazonKinesisReadOnlyAccess** 를 차례로 추가한 뒤 IAM 콘솔 페이지를 닫습니다.
    ![aws-lambda-iam-role-policies](/analytics-on-aws/images/aws-lambda-iam-role-policies.png)
    2. Memory와 Timeout을 알맞게 조정합니다. 이 실습에서는 Timout을 `5 min` 으로 설정합니다.
    ![](/analytics-on-aws/images/lambda-upsert-timeout.png)
    3. **Save** 를 클릭합니다.
13. VPC 패널에서 **\[Edit\]** 버튼을 클릭해서 Edit VPC 화면으로 이동합니다. 
    * Elasticsearch service의 도메인을 생성한 VPC와 subnets을 선택합니다.
    * Elasticsearch service 도메인에 접근이 허용된 `use-es-cluster-sg`
security groups을 선택합니다.
    ![](/analytics-on-aws/images/lambda-upsert-vpc.png)

1.  **Save** 버튼을 클릭하여 다시 Configuration 탭으로 돌아갑니다.
2.  Designer 패널에서 **\[Add trigger\]** 를 선택합니다.
3.  **Trigger configuration** 의 `Select a trigger` 에서 **Kinesis** 를 선택 합니다.
4.  Kinesis stream 에서 앞서 생성한 Kinesis Data Stream(`retail-trans`)을 선택합니다. 나머지는 기본 설정을 유지합니다.
    ![](/analytics-on-aws/images/lambda-upsert-trigger.png)

5.  **\[Add\]** 를 선택합니다.
![aws-lambda-kinesis](/analytics-on-aws/images/aws-lambda-kinesis.png)

