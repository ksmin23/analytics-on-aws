---
title: "Appendix"
weight: 900
pre: "<b>7. </b>"
---

{{% notice note %}}
At the end of this lab, you should delete the resources you used to avoid incurring additional charges for the AWS account you used.
{{% /notice %}}

Introducing how to deploy using the AWS CDK.

### Prerequisites
1. Install AWS CDK Toolkit.

    ```shell script
    npm install -g aws-cdk
    ```

2. Verify that cdk is installed properly by running the following command:
    ```
    cdk --version
    ```
    ex)
    ```shell script
    $ cdk --version
    1.51.0 (build 8c2d53c)
    ```

##### Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

### Deployment

When deployed as CDK, `1(a), 1(b), 1(c), 1(f), 2(b), 2(a)` in the architecture diagram below are automatically created.

![aws-analytics-system-build-steps-extra](/analytics-on-aws/images/aws-analytics-system-build-steps-extra.png)

1. Refer to [Getting Started With the AWS CDK](https://docs.aws.amazon.com/cdk/latest/guide/getting_started.html) to install cdk. 
Create an IAM User to be used when running cdk and register it in `~/.aws/config`.For example, after creating an IAM User called cdk_user, add it to `~/.aws/config` as shown below.

    ```shell script
    $ cat ~/.aws/config
    [profile cdk_user]
    aws_access_key_id=AKIAI44QH8DHBEXAMPLE
    aws_secret_access_key=je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
    region=us-east-1
    ```

1. Create a Python package to register in the Lambda Layer and store it in the s3 bucket. For example, create an s3 bucket named `lambda-layer-resources` so that you can save the elasticsearch package to register in the Lambda Layer as follows.

    ```shell script
    $ aws s3 ls s3://lambda-layer-resources/var/
    2019-10-25 08:38:50          0
    2019-10-25 08:40:28    1294387 es-lib.zip
    ```

2. After downloading the source code from git, enter the s3 bucket name where the package to be registered in the lambda layer is stored in an environment variable called `S3_BUCKET_LAMBDA_LAYER_LIB`.
After setting, deploy using the `cdk deploy` command.

    ```shell script
    $ git clone https://github.com/ksmin23/aws-analytics-immersion-day.git
    $ cd aws-analytics-immersion-day
    $ python3 -m venv .env
    $ source .env/bin/activate
    (.env) $ pip install -r requirements.txt
    (.env) $ export CDK_DEFAULT_ACCOUNT=$(aws sts get-caller-identity --query Account --output text)
    (.env) $ export CDK_DEFAULT_REGION=us-west-2
    (.env) $ cdk bootstrap aws://${CDK_DEFAULT_ACCOUNT}/${CDK_DEFAULT_REGION}
    (.env) $ export S3_BUCKET_LAMBDA_LAYER_LIB=lambda-layer-resources
    (.env) $ cdk --profile cdk_user deploy
    ```

1. To delete the deployed application, execute the `cdk destroy` command as follows.
    ```shell script
    (.env) $ cdk --profile cdk_user destroy
    ```
