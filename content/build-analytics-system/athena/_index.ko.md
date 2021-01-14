---
title: "Athena"
weight: 34
pre: "<b>3-4. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.svg)

## Athena를 이용해서 데이터 분석 하기

Amazon Athena를 이용해서 S3에 저장된 데이터를 기반으로 테이블을 만들고, 테이블을 쿼리한 다음 쿼리 결과를 확인할 수 있습니다.
먼저 데이터를 쿼리하기 위해서 데이터베이스를 생성합니다.

### 1단계: 데이터베이스 생성
1. [Athena 콘솔](https://console.aws.amazon.com/athena/home)을 엽니다.
2. Query editor 페이지가 열립니다. 우측 상단의 **Setting** 를 클릭해서 Athena의 쿼리 결과를 저장할 s3 위치를 설정합니다.
![aws-athena-setup-query-results-location-01](/analytics-on-aws/images/aws-athena-setup-query-results-location-01.png)
이번 실습에서는 Kinesis Data Firehose 설정 단계에서 생성한 s3 bucket에 Athena의 쿼리 결과를 저장할 디렉터리를 생성합니다.
예를 들어, `s3://aws-analytics-immersion-day-xxxxxxxx/athena-query-results/` (`xxxxxxxx` 는 bucket 이름이 겹치지 않도록 입력한 임의의 숫자나
문자열 입니다.)
![aws-athena-setup-query-results-location-02](/analytics-on-aws/images/aws-athena-setup-query-results-location-02.png)

1. Athena 쿼리 편집기에서 다음 CREATE DATABASE 문을 입력한 다음, **\[Run Query\]** 버튼을 클릭합니다.
    ```buildoutcfg
    CREATE DATABASE mydatabase
    ```
2. 카탈로그 디스플레이가 새로 고쳐지고 왼쪽 **\[Catalog\]** 대시보드의 **\[DATABASE\]** 목록에 `mydatabase`가 표시되는지 확인합니다.

    ![aws-athena-db](/analytics-on-aws/images/aws-athena-db.png)

### 2단계: 테이블 생성
1. **\[DATABASE\]** 에 `mydatabase`가 선택되었는지 확인한 후 **\[New Query\]** 를 선택합니다.
2. 쿼리 창에 다음 CREATE TABLE 문을 입력한 후 `LOCATION` 부분의 버킷 이름을 본인 버킷 이름으로 변경하십시오.
    ```buildoutcfg
    CREATE EXTERNAL TABLE `mydatabase.retail_trans_json`(
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
      'org.openx.data.jsonserde.JsonSerDe' 
    STORED AS INPUTFORMAT 
      'org.apache.hadoop.mapred.TextInputFormat' 
    OUTPUTFORMAT 
      'org.apache.hadoop.hive.ql.io.IgnoreKeyTextOutputFormat'
    LOCATION
      's3://aws-analytics-immersion-day-xxxxxxxx/json-data'
    ```

    알맞게 변경하였다면 **\[Run Query\]** 버튼을 클릭합니다.
    테이블 `retail_trans_json`가 생성되고 데이터베이스의 **\[Catalog\]** 대시보드에 표시됩니다. 좌측 패널의 Tables 리스트를 확인하십시오.

3. 테이블을 생성한 이후 **\[New Query\]** 를 선택하고 다음을 실행해서, 파티션의 데이터를 로드합니다.
    ```buildoutcfg
    MSCK REPAIR TABLE mydatabase.retail_trans_json
    ```

### 3단계: 데이터 쿼리
1. **\[New Query\]** 를 선택하고 쿼리 창의 아무 곳에나 다음 쿼리문을 입력한 다음 **\[Run Query\]** 를 선택합니다.
    ```buildoutcfg
    SELECT *
    FROM retail_trans_json
    LIMIT 10
    ```
    다음과 같은 형식의 결과가 반환됩니다.
    ![aws_athena_select_all_limit_10](/analytics-on-aws/images/aws_athena_select_all_limit_10.png)
