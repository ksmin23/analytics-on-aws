---
title: "Athena"
weight: 34
pre: "<b>3-4. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## Analyzing data using Athena

Using **Amazon Athena**, you can create tables based on data stored in S3, query tables, and view query results.
First, create a database to query the data.

### Step 1: Create a database
1. Open Athena console.
2. The first time you visit Athena console, you will be taken to the **\[Get Started\]** page. Select **\[Get Started\]** to open the query editor.
3. If this is your first time visiting, click **set up a query result location in Amazon S3** to set the s3 location to save Athena's query results.
![aws-athena-setup-query-results-location-01](/analytics-on-aws/images/aws-athena-setup-query-results-location-01.png)
In this lab, we will create a directory to store Athena's query results in the s3 bucket created in **Kinesis Data Firehose** setup step.
For example, `s3://aws-analytics-immersion-day-xxxxxxxx/athena-query-results/` (`xxxxxxxx` is random numbers or characters entered so that the bucket names do not overlap)
![aws-athena-setup-query-results-location-02](/analytics-on-aws/images/aws-athena-setup-query-results-location-02.png)
Unless you are visiting for the first time, Athena Query Editor is oppened.
4. You can see a query window with sample queries in the Athena Query Editor. Start typing your query anywhere in the query window.
5. To create a database called `mydatabase`, enter the following `CREATE DATABASE` statement, then select **\[Run Query\]**.
    ```buildoutcfg
    CREATE DATABASE mydatabase
    ```
6. Confirm that the the database **\[Catalog\]** refreshes and `mydatabase` is displayed in the **\[DATABASE\]** list on the left **\[Catalog\]** dashboard.

### Step 2: Create a table
1. Make sure that `mydatabase` is selected in **\[DATABASE\]**, and then select **\[New Query\]**.
2. Enter the following `CREATE TABLE` statement in the query window and select **\[Run Query\]**.
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
    The table `retail_trans_json` is created and displayed in the dashboard of the database **\[Catalog\]**.
3. After creating the table, select **\[New Query\]** and run the following to load the partition data.
    ```buildoutcfg
    MSCK REPAIR TABLE mydatabase.retail_trans_json
    ```

### Step 3: Query Data
1. Select **\[New Query\]**; enter the following query statement anywhere in the query window, then select **\[Run Query\]**.
    ```buildoutcfg
    SELECT *
    FROM retail_trans_json
    LIMIT 10
    ```
The result is returned in the following format:
![aws_athena_select_all_limit_10](/analytics-on-aws/images/aws_athena_select_all_limit_10.png)
