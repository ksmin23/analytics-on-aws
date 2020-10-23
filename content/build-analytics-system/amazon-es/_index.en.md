---
title: "Amazon Elasticsearch Service"
weight: 37
pre: "<b>3-7. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## Create Amazon Elasticsearch Service for Real-Time Data Analysis

An Elasticsearch cluster is created to store and analyze data in real time. Amazon ES domains are synonymous with Elasticsearch clusters. A domain is a setting that specifies a setting, instance type, number of instances, and storage resources.

1. In the AWS Management Console, select the **Elasticsearch** service in the Analytics category.
2. (Step 1: Choose deployment type) Select **Create a new domain**.
3. On the **Create Elasticsearch domain** page, select **Production** for **Deployment type**.
4. For **Version**, choose the Elasticsearch version for your domain. We recommend that you choose the latest supported version. For more information, see [Supported Elasticsearch Versions](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/what-is-amazon-elasticsearch-service.html#aes-choosing-version).
5. Click **\[Next\]**.
6. (Step 2: Configure domain) Enter the name of the domain. In this lab, `retail` will be used as the example domain name.
7. For **Instance type**, choose the instance type of your Amazon ES domain. In this lab, it is recommended to use a small, economical instance type (`t2.medium.elasticsearch`) suitable for testing purposes.
8. Enter the desired number of instances in **Number of nodes**. In this lab, we will use the default value of `1`.
9. Select EBS for **Data nodes storage type**.
    + a. Select General Purpose (SSD) for the **EBS volume type**. For more information, see Amazon EBS Volume Types.
    + b. In EBS volume size, enter the **EBS storage size per node** for each data node in GiB. In this lab, we will use the default value of `10`.
10. For now, you can ignore the **Dedicated master nodes, Snapshot configuration** and **Optional Elasticsearch cluster settings** sections.
11. Click **\[Next\]**.
12. (Step 3: Configure access and security) For **Network configuration**, select **VPC access**. Choose the appropriate VPC and subnet. Select the `es-cluster-sg` created in the preparation step as Security Groups.
13. For now, disable **Amazon Cognito Authentication** and **Fine–grained access control**.
14. For **Access policy**, select **JSON defined access policy** from **Domain access policy**, and then create and enter a **JSON defined access policy** using the following template in **Add or edit the access policy**.
    + JSON defined access policy Template - Enter the domain name entered in **(Step 2: Configure domain)** in `<DOMAIN-NAME>`.
        ```json
        {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Effect": "Allow",
              "Principal": {
                "AWS": "*"
              },
              "Action": [
                "es:Describe*",
                "es:List*",
                "es:Get*",
                "es:ESHttp*"
              ],
              "Resource": "arn:aws:es:<region-id>:<account-id>:domain/<DOMAIN-NAME>/*"
            }
          ]
        }
        ```
    + ex) In this lab, we used `retail` as the domain name, so we create a JSON defined access policy as shown below.
        ```json
        {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Effect": "Allow",
              "Principal": {
                "AWS": "*"
              },
              "Action": [
                "es:Describe*",
                "es:List*",
                "es:Get*",
                "es:ESHttp*"
              ],
              "Resource": "arn:aws:es:us-west-2:109624076471:domain/retail/*"
            }
          ]
        }
        ```
15. **Encryption** only allows **Require HTTPS for all traffic to the domain**, and other items are disabled.
16. Keep all default values ​​of **Encryption**. Select **\[Next\]**.
17. On the **Review** page, review your domain configuration and then choose **Confirm**.

