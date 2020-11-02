---
title: "AWS Lambda"
weight: 38
pre: "<b>3-8. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## Ingest real-time data into ElasticSearch using AWS Lambda Functions

You can index data into Amazon Elasticsearch Service in real time using a Lambda function.
In this lab, you will create a Lambda function using the AWS Lambda console.

### To add a common library to Layers for use by Lambda functions,
1. Open the **AWS Lambda Console**.
2. Enter the **Layers** menu and select **\[Create layer\]**.
3. Enter `es-lib` for the Name.
4. Select `Upload a file from Amazon S3` and enter the s3 link url where the library code is stored or the compressed library code file.
In this lab, we will use the `resources/es-lib.zip` file. For how to create `es-lib.zip`, refer to [Example of creating a Python package to register in AWS Lambda Layer](/en/reference/).
5. Select `Python 3.8` from `Compatible runtimes`.
![aws-lambda-create-layer](/analytics-on-aws/images/aws-lambda-create-layer.png)

### To create a Lambda function,
1. Open the **AWS Lambda Console**.
2. Select **\[Create a function\]**.
3. Enter `UpsertToES` for Function name.
4. Select `Python 3.8` in Runtime.
5. Select **\[Create a function\]**.
![aws-lambda-create-function](/analytics-on-aws/images/aws-lambda-create-function.png)
6. Select **\[Add trigger\]** in the Designer tab. In Layers, choose Add a layer.
7. Click `Select from list of runtime compatible layers` in Layer selection, and select Name and Version of the previously created layer as Name and Version in Compatiable layers.
![aws-lambda-add-layer-to-function](/analytics-on-aws/images/aws-lambda-add-layer-to-function.png)
If the runtime version of Layer and the runtime of Lambda function are different, the layers required for **Compatiable layers** of **Layer selection** may not be displayed in the list.
In this case, click `Provide a layer version ARN` in **Layer selection**, and enter arn of Layer directly.
![aws-lambda-add-layer-to-function-layer-version-arn](/analytics-on-aws/images/aws-lambda-add-layer-to-function-layer-version-arn.png)
8. Click **\[Add\]**.
9. Select `UpsertToES` in the Designer tab to return to Function code and Configuration.
10. Copy and paste the code from the `upsert_to_es.py` file into the code editor of the Function code.
11. In Environment variables, click **\[Edit\]**.
12. Click **\[Add environment variables\]** to register the following 4 environment variables.
    ```shell script
    ES_HOST=<elasticsearch service domain>
    ES_INDEX=<elasticsearch index name>
    ES_TYPE=<elasticsearch type name>
    REQUIRED_FIELDS=<primary key로 사용될 column 목록>
    REGION_NAME=<region-name>
    DATE_TYPE_FIELDS=<column 중, date 또는 timestamp 데이터 타입의 column>
    ```
    For example, set Environment variables as follows:
    ```buildoutcfg
    ES_HOST=vpc-retail-xkl5jpog76d5abzhg4kyfilymq.us-west-1.es.amazonaws.com
    ES_INDEX=retail
    ES_TYPE=trans
    REQUIRED_FIELDS=Invoice,StockCode,Customer_ID
    REGION_NAME=us-west-2
    DATE_TYPE_FIELDS=InvoiceDate
    ```
13. Click **\[Save\]**.
14. In order to execute the lambda function in the VPC and read data from Kinesis Data Streams, you need to add the IAM Policy required for the Execution role required to execute the lamba function.
Click `View the UpsertToES-role-XXXXXXXX role on the IAM console.` to edit the IAM Role.
![aws-lambda-execution-iam-role](/analytics-on-aws/images/aws-lambda-execution-iam-role.png)
15. After clicking the **\[Attach policies\]** button in the **\[Permissions\]** tab of IAM Role, add **AWSLambdaVPCAccessExecutionRole** and **AmazonKinesisReadOnlyAccess** in order.
![aws-lambda-iam-role-policies](/analytics-on-aws/images/aws-lambda-iam-role-policies.png)
16. Click the **\[Edit\]** button in the VPC category to go to the Edit VPC screen. Select `Custom VPC` for VPC connection.
Choose the VPC and subnets where you created the domain for the Elasticsearch service, and choose the security groups that are allowed access to the Elasticsearch service domain.
17. Select **\[Edit\]** in Basic settings. Adjust Memory and Timeout appropriately. In this lab, we set Timout to `5 min`.
18. Go back to the Designer tab and select **\[Add trigger\]**.
19. Select **Kinesis** from `Select a trigger` in the **Trigger configuration**.
20. Select the Kinesis Data Stream (`retail-trans`) created earlier in **Kinesis stream**.
21. Click **\[Add\]**.
![aws-lambda-kinesis](/analytics-on-aws/images/aws-lambda-kinesis.png)
