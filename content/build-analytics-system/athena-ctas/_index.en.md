---
title: "Athena CTAS"
weight: 36
pre: "<b>3-6. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps-extra.png)

## Combine small files stored in S3 into large files using AWS Lambda Function

When real-time incoming data is stored in S3 using Kinesis Data Firehose, files with small data size are created.
To improve the query performance of Amazon Athena, it is recommended to combine small files into one large file.
To run these tasks periodically, I want to create an AWS Lambda function function that executes Athena's Create Table As Select (CTAS) query.

1. Open the **AWS Lambda Console**.
2. Select **\[Create a function\]**.
3. Enter `MergeSmallFiles` for Function name.
4. Select `Python 3.8` in Runtime.
5. Select **\[Create a function\]**.
![aws-athena-ctas-lambda-create-function](/analytics-on-aws/images/aws-athena-ctas-lambda-create-function.png)
6. Select **\[Add trigger\]** in the Designer tab.
7. Select **CloudWatch Events/EventBridge** in `Select a trigger` of **Trigger configuration**.
Select `Create a new rule` in Rule and enter the appropriate rule name (eg `MergeSmallFilesEvent`) in Rule name.
Select `Schedule expression` as the rule type, and enter `cron(5 * * * *)` for running the task every 5 minutes in the schedule expression.
![aws-athena-ctas-lambda-add-trigger](/analytics-on-aws/images/aws-athena-ctas-lambda-add-trigger.png)
8. In **Trigger configuration**, click **\[Add\]**.
9. Copy and paste the code from the `athena_ctas.py` file into the code editor of the Function code.
10. Click **\[Add environment variables\]** to register the following environment variables.
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
    For example, set Environment variables as follows:
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
11. To add the IAM Policy required to execute Athena queries, click `View the MergeSmallFiles-role-XXXXXXXX role on the IAM console.` in the Execution role and modify the IAM Role.
![aws-athena-ctas-lambda-execution-iam-role](/analytics-on-aws/images/aws-athena-ctas-lambda-execution-iam-role.png)
12. After clicking the **\[Attach policies\]** button in the **\[Permissions\]** tab of IAM Role, add **AmazonAthenaFullAccess** and **AmazonS3FullAccess** in order.
![aws-athena-ctas-lambda-iam-role-policies](/analytics-on-aws/images/aws-athena-ctas-lambda-iam-role-policies.png)
13. Select **\[Edit\]** in Basic settings. Adjust Memory and Timeout appropriately. In this lab, we set Timout to `5 min`.
