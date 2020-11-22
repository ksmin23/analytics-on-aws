---
title: "Athena CTAS"
weight: 36
pre: "<b>3-6. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps-extra.png)

## AWS Lambda Function을 이용해서 S3에 저장된 작은 파일들을 큰 파일로 합치기

실시간으로 들어오는 데이터를 Kinesis Data Firehose를 이용해서 S3에 저장할 경우, 데이터 사이즈가 작은 파일들이 생성됩니다.
Amazon Athena의 쿼리 성능 향상을 위해서 작은 파일들을 하나의 큰 파일로 합쳐주는 것이 좋습니다.
이러한 작업을 주기적으로 실행하기 위해서Athena의 CTAS(Create Table As Select) 쿼리를 실행하는 AWS Lambda function 함수를 생성하고자 합니다.

### 1단계: CTAS 쿼리 결과를 저장하는 테이블 생성하기
1. **Athena 콘솔** 에 접속해서 Athena 쿼리 편집기로 이동합니다.
2. **\[DATABASE\]** 에서 mydatabase를 선택하고, **\[New Query\]** 를 선택합니다.
3. 쿼리 창에 다음 CREATE TABLE 문을 입력한 후 **\[Run Query\]** 를 선택합니다.<br/>
이번 실습에서는 `retal_tran_json` 테이블의 json 포맷 데이터를 parquet 포맷으로 변경해서 `ctas_retail_trans_parquet` 이라는 테이블에 저장할 것 입니다.<br/>
`ctas_retail_trans_parquet` 테이블의 데이터는 앞서 생성한 S3 bucket의 `s3://aws-analytics-immersion-day-xxxxxxxx/parquet-retail-trans` 위치에 저장할 것 입니다.
    ```buildoutcfg
    CREATE EXTERNAL TABLE `mydatabase.ctas_retail_trans_parquet`(
      `invoice` string COMMENT 'Invoice number', 
      `stockcode` string COMMENT 'Product (item) code', 
      `description` string COMMENT 'Product (item) name', 
      `quantity` int COMMENT 'The quantities of each product (item) per transaction', 
      `invoicedate` timestamp COMMENT 'Invoice date and time', 
      `price` float COMMENT 'Unit price', 
      `customer_id` string COMMENT 'Customer number', 
      `country` string COMMENT 'Country name')
    PARTITIONED BY ( 
      `year` int, 
      `month` int, 
      `day` int, 
      `hour` int)
    ROW FORMAT SERDE 
      'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe' 
    STORED AS INPUTFORMAT 
      'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat' 
    OUTPUTFORMAT 
      'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
    LOCATION
      's3://aws-analytics-immersion-day-xxxxxxxx/parquet-retail-trans'
    TBLPROPERTIES (
      'has_encrypted_data'='false', 
      'parquet.compression'='SNAPPY')
    ;
    ```

### 2단계: AWS Lambda 함수 생성하기
1. **AWS Lambda 콘솔** 을 엽니다.
2. **\[Create a function\]** 을 선택합니다.
3. Function name(함수 이름)에 `MergeSmallFiles` 을 입력합니다.
4. Runtime 에서 `Python 3.8` 을 선택합니다.
5. **\[Create a function\]** 을 선택합니다.
![aws-athena-ctas-lambda-create-function](/analytics-on-aws/images/aws-athena-ctas-lambda-create-function.png)
6. Designer 탭에 **\[Add trigger\]** 를 선택합니다.
7. **Trigger configuration** 의 `Select a trigger` 에서 **CloudWatch Events/EventBridge** 를 선택 합니다.
Rule에서 `Create a new rule` 선택하고, Rule name에 적절한 rule name(예: `MergeSmallFilesEvent`)을 입력 합니다.
Rule type으로 `Schedule expression`을 선택하고, Schedule expression에 매시각 5분 마다 작업이 실행되도록,
`cron(5 * * * *)` 입력합니다.
![aws-athena-ctas-lambda-add-trigger](/analytics-on-aws/images/aws-athena-ctas-lambda-add-trigger.png)
8. **Trigger configuration** 에서 **\[Add\]** 를 클릭합니다.
9. Function code의 코드 편집기에 `athena_ctas.py` 파일의 코드를 복사해서 붙여넣습니다.
10. **\[Add environment variables\]** 를 클릭해서 다음 Environment variables을 등록합니다.
    ```shell script
    OLD_DATABASE=<source database>
    OLD_TABLE_NAME=<source table>
    NEW_DATABASE=<destination database>
    NEW_TABLE_NAME=<destination table>
    WORK_GROUP=<athena workgroup>
    OUTPUT_PREFIX=<destination s3 prefix>
    STAGING_OUTPUT_PREFIX=<staging s3 prefix used by athena>
    COLUMN_NAMES=<columns of source table excluding partition keys>
    ```
    예를 들어, 다음과 같이 Environment variables을 설정합니다.
    ```buildoutcfg
    OLD_DATABASE=mydatabase
    OLD_TABLE_NAME=retail_trans_json
    NEW_DATABASE=mydatabase
    NEW_TABLE_NAME=ctas_retail_trans_parquet
    WORK_GROUP=primary
    OUTPUT_PREFIX=s3://aws-analytics-immersion-day-xxxxxxxx/parquet-retail-trans
    STAGING_OUTPUT_PREFIX=s3://aws-analytics-immersion-day-xxxxxxxx/tmp
    COLUMN_NAMES=invoice,stockcode,description,quantity,invoicedate,price,customer_id,country
    ```
11. Athena 쿼리를 수행하는데 필요한 IAM Policy를 추가하기 위해서 Execution role에서 
`View the MergeSmallFiles-role-XXXXXXXX role on the IAM console.` 을 클릭 해서 IAM Role을 수정합니다.
![aws-athena-ctas-lambda-execution-iam-role](/analytics-on-aws/images/aws-athena-ctas-lambda-execution-iam-role.png)
12. IAM Role의 **\[Permissions\]** 탭에서 **\[Attach policies\]** 버튼을 클릭 후, 
**AmazonAthenaFullAccess**, **AmazonS3FullAccess** 를 차례로 추가 합니다.
![aws-athena-ctas-lambda-iam-role-policies](/analytics-on-aws/images/aws-athena-ctas-lambda-iam-role-policies.png)
13. Basic settings에서 **\[Edit\]** 선택합니다. Memory와 Timeout을 알맞게 조정합니다.
이 실습에서는 Timout을 `5 min` 으로 설정합니다.

