---
title: "Verify"
weight: 33
pre: "<b>3-3. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.svg)

## Verify data pipeline operation

Using the sample data, check whether data is normally collected in `Kinesis Data Streams -> Kinesis Data Firehose -> S3`.

1. Connect SSH to the previously created E2 instance.
2. Run `gen_kinesis_data.py`.
    ```shell script
    $ python3 gen_kinesis_data.py --help
    usage: gen_kinesis_data.py [-h] [-I INPUT_FILE] [--out-format {csv,tsv,json}]
                               [--service-name {kinesis,firehose}]
                               [--stream-name STREAM_NAME] [--max-count MAX_COUNT]
                               [--dry-run]
    
    optional arguments:
      -h, --help            show this help message and exit
      -I INPUT_FILE, --input-file INPUT_FILE
                            The input file path ex)
                            ./resources/online_retail.csv
      --out-format {csv,tsv,json}
      --service-name {kinesis,firehose}
      --stream-name STREAM_NAME
                            The name of the stream to put the data record into.
      --max-count MAX_COUNT
                            The max number of records to put.
      --dry-run
    
    $ python3 gen_kinesis_data.py -I resources/online_retail.csv \
    --service-name kinesis \
    --out-format json \
    --stream-name retail-trans
    ```
3. Verify that data is generated every second. Proceed to the next step while running for sufficient data collection.
4. If you check the S3 bucket you created a few minutes later, you can see that the original data generated is stored on S3 via **Kinesis Data Firehose**.
