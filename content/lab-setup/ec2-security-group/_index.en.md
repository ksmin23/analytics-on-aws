---
title: "Creating Security Groups"
weight: 22
pre: "<b>2-2 </b>"
tag:
  - Security_Group
---

## bastion host로 사용할 EC2 인스턴스를 위한 Security Groups 생성
실습용 EC2 인스턴에서 사용할 security group을 생성하고 구성합니다.

1. AWS Management Console에서 EC2 서비스에 접속합니다.
2. **NETWORK & SECURITY** 메뉴에서 **Security Groups** 항목을 선택합니다.
3. **\[Create Security Group\]** 을 클릭합니다.
4. Create Security Group 화면에서 Security Group에 필요한 정보를 입력한 후, 새로운 security group을  **\[Create\]** 합니다.
    + Security group name : bastion
    + Description : security group for bastion
 
    Security group rules의 **Inbound** 에 아래 내용을 입력합니다.
    + Type : SSH
    + Protocol : TCP
    + Port Range : 22
    + Source : 0.0.0.0/0

    ![aws-ec2-security-group-for-bastion](/analytics-on-aws/images/aws-ec2-security-group-for-bastion.png)

## Elasicsearch Service에서 사용할 Security Groups 생성
Elasticsearch Service을 위한 security group을 생성하고 구성합니다.
1. AWS Management Console에서 EC2 서비스에 접속합니다.
2. **NETWORK & SECURITY** 메뉴에서 **Security Groups** 항목을 선택합니다.
3. **\[Create Security Group\]** 을 클릭합니다.
4. Create Security Group 화면에서 Security Group에 필요한 정보를 입력한 후, 새로운 security group을  **\[Create\]** 합니다.
    + Security group name : use-es-cluster-sg
    + Description : security group for an es client

    Security group rules의 **Inbound** 은 아무것도 입력하지 않습니다.
    
    ![aws-ec2-security-group-for-es-client](/analytics-on-aws/images/aws-ec2-security-group-for-es-client.png)
5. 다시 **\[Create Security Group\]** 클릭해서 Create Security Group 화면으로 이동합니다.
Security Group에 필요한 정보를 입력한 후, 새로운 security group을 **\[Create\]** 합니다.
    + Security group name : es-cluster-sg
    + Description : security group for an es cluster
 
    Security group rules의 **Inbound** 에 아래 내용을 입력합니다.
    + Type : All TCP
    + Protocol : TCP
    + Port Range : 0-65535
    + Source : `use-es-cluster-sg` 의 security group id ex) sg-038b632ef1825cb7f

     ![aws-ec2-security-group-for-es-cluster](/analytics-on-aws/images/aws-ec2-security-group-for-es-cluster.png)
