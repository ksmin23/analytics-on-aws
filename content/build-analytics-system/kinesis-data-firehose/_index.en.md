---
title: "Kinesis Data Firehose"
weight: 32
pre: "<b>3-2. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## Create Kinesis Data Firehose to store data in S3

{{% notice info %}}
With Kinesis Data Firehose, you can collect data in real time to destinations such as S3, Redshift, and ElasticSearch.
{{% /notice %}}

Select Kinesis services the AWS Management Console.

1. Click **Get Started** button.
2. Click **\[Create delivery stream\]** on **Deliver streaming data with Kinesis Firehose delivery streams** menu.<br/>
Start creating a new Firehose delivery stream.
3. (Step 1: Name and source) Enter the desired name (e.g. `retail-trans`) in **Delivery stream name**.
4. **Next**를 클릭합니다.Select `Kinesis Data Stream` from **Choose a source**, select Kinesis Data Stream (eg `retail-trans`) created earlier, and then click **Next**.
5. (Step 2: Process records) For **Transform source records with AWS Lambda / Convert record format**, both select the default option `Disabled` and click **Next**.
6. (Step 3: Choose a destination) Select Amazon S3 as **Destination** and click `Create new` to create an S3 bucket.
The S3 bucket name is in the format of `aws-analytics-immersion-day-xxxxxxxx` in this lab, and `xxxxxxxx` is random numbers or characters so that the S3 bucket names do not overlap.

    Enter S3 prefix. For example, type as follows:    
    ```buildoutcfg
    json-data/year=!{timestamp:yyyy}/month=!{timestamp:MM}/day=!{timestamp:dd}/hour=!{timestamp:HH}/
    ```

    Enter S3 error prefix. For example, type as follows:
    ```buildoutcfg
    error-json/year=!{timestamp:yyyy}/month=!{timestamp:MM}/day=!{timestamp:dd}/hour=!{timestamp:HH}/!{firehose:error-output-type}
    ```

    After entering S3 prefix and 3 error prefix, click Next. 
    (reference: [https://docs.aws.amazon.com/firehose/latest/dev/s3-prefixes.html](https://docs.aws.amazon.com/firehose/latest/dev/s3-prefixes.html))
7. (Step 4: Configure settings) Set buffer size to `1MB` and buffer interval to `60` seconds in **S3 buffer conditions**.
8. Click the **\[Create new, or Choose\]** button in the IAM role below.
9. In the newly opened tab, it automatically creates the IAM role `firehose_delivery_role` with the required policy. Click **Allow** button to proceed.
![kfh_create_new_iam_role](/analytics-on-aws/images/kfh_create_new_iam_role.png)
10. After confirming that the newly created role has been added, click **Next** button.
11. (Step 5: Review) If there are no errors after checking the information entered in **Review**, click the **\[Create delivery stream\]** button to complete the **Firehose** creation.
