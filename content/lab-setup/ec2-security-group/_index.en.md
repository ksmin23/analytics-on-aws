---
title: "Creating Security Groups"
weight: 22
pre: "<b>2-1 </b>"
tag:
  - Security_Group
---

## Security Groups to create an EC2 instance for a bastion host
Create and configure a security group of EC2 instance.

1. Click this link to create Security Group.
2. In **Create Security Group** screen, enter the necessary information for the **Security Group**, and then **\[Create\]** a new security group.
    + Security group name : bastion
    + Description : security group for bastion
 
    Create the following **Inbound** Rules.
    + Type : SSH
    + Protocol : TCP
    + Port Range : 22
    + Source : 0.0.0.0/0

    ![aws-ec2-security-group-for-bastion](/analytics-on-aws/images/aws-ec2-security-group-for-bastion.png)

## Security Groups created for use in Elasticsearch Service
Create and configure a security group for Elasticsearch Service.

1. Connect to EC2 service in AWS Management Console.
2. Select the **Security Groups** item from the **NETWORK & SECURITY** menu.
3. Click **\[Create Security Group\]**.
4. On the **Create Security Group** screen, enter the necessary information for the Security Group, and then **\[Create\]** a new security group.
    + Security group name : use-es-cluster-sg
    + Description : security group for an es client

    Enter nothing in **Inbound** of the security group rules.
    
    ![aws-ec2-security-group-for-es-client](/analytics-on-aws/images/aws-ec2-security-group-for-es-client.png)
5.  Click **\[Create Security Group\]** again to go to the **Create Security Group** screen. After entering the necessary information for the security group, **\[Create\]** a new security group.
    + Security group name : `es-cluster-sg`
    + Description : `security group for an es cluster`
    + VPC: Default
 
    Create the following **Inbound** Rules.
    + Type : All TCP
    + Protocol : TCP
    + Port Range : 0-65535
    + Source : `use-es-cluster-sg` 의 security group id ex) sg-038b632ef1825cb7f

     ![aws-ec2-security-group-for-es-cluster](/analytics-on-aws/images/aws-ec2-security-group-for-es-cluster.png)