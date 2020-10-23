---
title: "Reference"
weight: 100
pre: "<b>6. </b>"
---

### AWS Developer Guide By Services
+ [Amazon Simple Storage Service (Amazon S3)](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html)
+ [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/what-is.html)
+ [Amazon Elasticsearch Service](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/what-is-amazon-elasticsearch-service.html)
+ [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
+ [Amazon Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html)
+ [Amazon Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/introduction.html)
+ [Amazon QuickSight](https://docs.aws.amazon.com/quicksight/latest/user/welcome.html)
+ [AWS Lambda Layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html#configuration-layers-path)
    + Example of creating a Python package to register with AWS Lambda layer: **elasticsearch** 
    ```
    $ python3 -m venv es-lib
    $ cd es-lib
    $ source bin/activate
    $ mkdir -p python_modules
    $ pip install elasticsearch -t python_modules
    $ mv python_modules python
    $ zip -r es-lib.zip python/
    $ aws s3 cp es-lib.zip s3://my-lambda-layer-packages/python/
    ```

### Further readings
##### Amazon S3
+ [New – Automatic Cost Optimization for Amazon S3 via Intelligent Tiering](https://aws.amazon.com/ko/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/)

##### Amazon Athena
+ [Top 10 Performance Tuning Tips for Amazon Athena](https://aws.amazon.com/ko/blogs/big-data/top-10-performance-tuning-tips-for-amazon-athena/)
+ [Extract, Transform and Load data into S3 data lake using CTAS and INSERT INTO statements in Amazon Athena](https://aws.amazon.com/ko/blogs/big-data/extract-transform-and-load-data-into-s3-data-lake-using-ctas-and-insert-into-statements-in-amazon-athena/)
+ [Query Amazon S3 analytics data with Amazon Athena](https://aws.amazon.com/blogs/storage/query-amazon-s3-analytics-data-with-amazon-athena/)

##### Amazon Elasticsearch Service
+ [Elasticsearch tutorial: a quick start guide](https://aws.amazon.com/blogs/database/elasticsearch-tutorial-a-quick-start-guide/)
+ [Run a petabyte scale cluster in Amazon Elasticsearch Service](https://aws.amazon.com/blogs/database/run-a-petabyte-scale-cluster-in-amazon-elasticsearch-service/)
+ [Analyze user behavior using Amazon Elasticsearch Service, Amazon Kinesis Data Firehose and Kibana](https://aws.amazon.com/blogs/database/analyze-user-behavior-using-amazon-elasticsearch-service-amazon-kinesis-data-firehose-and-kibana/)

##### AWS Lambda
+ [Introduction to Messaging for Modern Cloud Architecture](https://aws.amazon.com/blogs/architecture/introduction-to-messaging-for-modern-cloud-architecture/)
+ [Understanding the Different Ways to Invoke Lambda Functions](https://aws.amazon.com/blogs/architecture/understanding-the-different-ways-to-invoke-lambda-functions/)

##### Amazon Kinesis Data Firehose
+ [Amazon Kinesis Data Firehose custom prefixes for Amazon S3 objects](https://aws.amazon.com/blogs/big-data/amazon-kinesis-data-firehose-custom-prefixes-for-amazon-s3-objects/)
+ [Amazon Kinesis Firehose Data Transformation with AWS Lambda](https://aws.amazon.com/blogs/compute/amazon-kinesis-firehose-data-transformation-with-aws-lambda/)

##### Amazon Kinesis Data Streams
+ [Under the hood: Scaling your Kinesis data streams](https://aws.amazon.com/blogs/big-data/under-the-hood-scaling-your-kinesis-data-streams/)
+ [Scale Amazon Kinesis Data Streams with AWS Application Auto Scaling](https://aws.amazon.com/blogs/big-data/scaling-amazon-kinesis-data-streams-with-aws-application-auto-scaling/)

##### Amazon Kinesis Data Analytics
+ [Streaming ETL with Apache Flink and Amazon Kinesis Data Analytics](https://aws.amazon.com/ko/blogs/big-data/streaming-etl-with-apache-flink-and-amazon-kinesis-data-analytics/)

##### Amazon QuickSight
+ [10 visualizations to try in Amazon QuickSight with sample data](https://aws.amazon.com/blogs/big-data/10-visualizations-to-try-in-amazon-quicksight-with-sample-data/)
+ [Visualize over 200 years of global climate data using Amazon Athena and Amazon QuickSight](https://aws.amazon.com/blogs/big-data/visualize-over-200-years-of-global-climate-data-using-amazon-athena-and-amazon-quicksight/)
+ [Advanced analytics with table calculations in Amazon QuickSight](https://aws.amazon.com/ko/blogs/big-data/advanced-analytics-with-table-calculations-in-amazon-quicksight/)

##### Etc
+ [Optimize downstream data processing with Amazon Kinesis Data Firehose and Amazon EMR running Apache Spark](https://aws.amazon.com/blogs/big-data/optimizing-downstream-data-processing-with-amazon-kinesis-data-firehose-and-amazon-emr-running-apache-spark/)
+ [Serverless Scaling for Ingesting, Aggregating, and Visualizing Apache Logs with Amazon Kinesis Firehose, AWS Lambda, and Amazon Elasticsearch Service](https://aws.amazon.com/blogs/database/serverless-scaling-for-ingesting-aggregating-and-visualizing-apache-logs-with-amazon-kinesis-firehose-aws-lambda-and-amazon-elasticsearch-service/)
+ [Analyze Apache Parquet optimized data using Amazon Kinesis Data Firehose, Amazon Athena, and Amazon Redshift](https://aws.amazon.com/blogs/big-data/analyzing-apache-parquet-optimized-data-using-amazon-kinesis-data-firehose-amazon-athena-and-amazon-redshift/)
+ [Our data lake story: How Woot.com built a serverless data lake on AWS](https://aws.amazon.com/blogs/big-data/our-data-lake-story-how-woot-com-built-a-serverless-data-lake-on-aws/)
