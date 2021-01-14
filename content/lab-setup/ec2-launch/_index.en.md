---
title: "Launch an EC2 Instance"
weight: 23
pre: "<b>2-2 </b>"
tag:
  - EC2_Launch
---

{{% notice info %}}
For this lab, we will use the **us-west-1 (Oregon) region**.
{{% /notice %}}

Create an EC2 instance that will generate the data needed for the lab in real time.

1. Click [this link](https://console.aws.amazon.com/ec2/v2/home#LaunchInstanceWizard:) to start creating a new instance.

2. Step 1: On the **Choose an Amazon Machine Image (AMI)** screen, choose **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![aws-ec2-choose-ami](/analytics-on-aws/images/aws-ec2-choose-ami.png)
5. Step 2: On the **Choose an Instance Type** screen, select t2.micro as the instance type. Click **\[Next: Configure Instance Details\]**.
![aws-ec2-choose-instance-type](/analytics-on-aws/images/aws-ec2-choose-instance-type.png)
6. Step 3: On the **Configure Instance Details** screen, select **Enable** for **Auto-assign Public IP**, and click **\[Next: Add Storage\]**.
![aws-ec2-configure-instance-details](/analytics-on-aws/images/aws-ec2-configure-instance-details.png)
7. Step 4: On the **Add Storage** screen, leave the defaults and click **\[Next: Add Tags\]**.
8. Step 5: On the **Add Tags** screen, click **\[Next: Configure Security Group\]**.
9. Step 6: On the **Configure Security Group** screen, select **Select an existing security group** from **Assign a security group**, and then select `bastion` and `use-es-cluster-sg` from the **Security Group** and click **\[Review and Launch\]**.
![aws-ec2-configure-security-group](/analytics-on-aws/images/aws-ec2-configure-security-group.png)
10. Step 7: click **\[Launch\]** on the **Review Instance Launch** screen. 
11. Create a key pair to access EC2 Instance.
Select Create a new key pair, enter `analytics-hol` as the Key pair name, and click **Download Key Pair**.
Save the Key Pair to any location on your PC and click **\[Launch Instances\]**. (EC2 Instance startup may take several minutes.)
![aws-ec2-select-keypair](/analytics-on-aws/images/aws-ec2-select-keypair.png)
12. For MacOS users, Change the File Permission of the downloaded Key Pair file to 400.
    ```shell script
    $ chmod 400 ./analytics-hol.pem 
    $ ls -lat analytics-hol.pem 
    -r--------  1 ******  ******  1692 Jun 25 11:49 analytics-hol.pem
    ```

  For Windows OS users, Please refer to [Use PuTTY to connect to your Linux instance from Windows](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html).
