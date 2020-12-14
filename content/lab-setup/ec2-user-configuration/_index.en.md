---
title: "Configuring your EC2 Instance"
weight: 24
pre: "<b>2-4 </b>"
tag:
  - EC2_Configuration
---

Configure the EC2 instances to access and control other AWS resources as follows:

1. Go to [EC2 console](https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#Instances:), and click the instance you just created. Then click **Connect**.
    ![](/analytics-on-aws/images/ec2-connect.png)

2. You will see the connection option page like the following screen shot. Click Connect to ssh into the instance.

    ![](/analytics-on-aws/images/ec2-ssm.png)
3. Perform the following actions in order on the EC2 instance connected with ssh.

    (1) Download the source code. 
    ```shell script
    wget 'https://github.com/ksmin23/aws-analytics-immersion-day/archive/master.zip'
    ```
    (2) Extract the downloaded source code.
    ```shell script
    unzip -u master.zip
    ```
    (3) Grant execution authority to the practice environment setting script.
    ```shell script
    chmod +x ./aws-analytics-immersion-day-master/set-up-hands-on-lab.sh
    ```
    (4) Execute the setup script to set the lab environment.
    ```shell script
    ./aws-analytics-immersion-day-master/set-up-hands-on-lab.sh
    ```
    (5) Make sure the files necessary for the lab are normally created after running the configuration script. For example, check if the source code and necessary files exist as shown below.
    ![aws-ec2-setup-hands-on-lab](/analytics-on-aws/images/aws-ec2-setup-hands-on-lab.png)

4. Perform `aws configure` to access other AWS resources. At this time, the IAM User data created earlier is used.
Open the previously downloaded **.csv** file, check the `Access key ID` and `Secret access key`, and enter them.
    ```shell script
    $ aws configure
    AWS Access Key ID [None]: <Access key ID>
    AWS Secret Access Key [None]: <Secret access key>
    Default region name [None]: us-west-2
    Default output format [None]: 
    ```
4. If the setting is complete, the information entered as follows will be masked.
    ```shell script
    $ aws configure
    AWS Access Key ID [****************EETA]:
    AWS Secret Access Key [****************CixY]:
    Default region name [None]: us-west-2
    Default output format [None]: 
    ```
