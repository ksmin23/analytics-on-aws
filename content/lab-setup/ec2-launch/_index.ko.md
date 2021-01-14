---
title: "EC2 생성"
weight: 23
pre: "<b>2-2 </b>"
tag:
  - EC2_Launch
---

{{% notice info %}}
이 실습에서는 **us-west-1 (오레곤) 리전을 사용**합니다.
{{% /notice %}}

실습에 필요한 데이터를 실시간으로 발생시킬 EC2 인스턴스를 생성합니다.

1. [이 링크](https://console.aws.amazon.com/ec2/v2/home#LaunchInstanceWizard:)를 클릭하여 새로운 인스턴스 생성을 시작합니다. 이때 우측 상단에서 리전이 **us-west-2** 로 잘 설정되어 있는지 확인하십시오.

2. Step 1: Choose an Amazon Machine Image (AMI) 화면에서 **Amazon Linux 2 AMI (HVM), SSD Volume Type** 을 선택합니다.
![aws-ec2-choose-ami](/analytics-on-aws/images/aws-ec2-choose-ami.png)
5. Step 2: Choose an Instance Type 화면에서 인스턴스 타입은 t2.micro를 선택합니다. **\[Next: Configure Instance Details\]** 을 클릭합니다.
![aws-ec2-choose-instance-type](/analytics-on-aws/images/aws-ec2-choose-instance-type.png)
6. Step 3: Configure Instance Details 화면에서 **Auto-assign Public IP** 를 **Enable** 선택하고, **\[Next: Add Storage\]** 을 클릭합니다.
![aws-ec2-configure-instance-details](/analytics-on-aws/images/aws-ec2-configure-instance-details.png)
7. Step 4: Add Storage 화면에서 기본값을 그대로 두고 **\[Next: Add Tags\]** 를 클릭합니다.
8. Step 5: Add Tags 화면에서 **\[Next: Configure Security Group\]** 을 클릭합니다.
9. Step 6: Configure Security Group 화면에서 Assign a security group 에서 Select an **existing** security group를 선택하고,
Security Group 중에서 Name이 `bastion`과 `use-es-cluster-sg` 를 선택 한 후 **\[Review and Launch\]** 를 클릭합니다.
![aws-ec2-configure-security-group](/analytics-on-aws/images/aws-ec2-configure-security-group.png)
10. Step 7: Review Instance Launch 화면에서 **\[Launch\]** 를 클릭합니다.
11. EC2 Instance에 접속하기 위한 Key pair를 생성합니다. 
Create a new key pair를 선택하고 Key pair name은 `analytics-hol` 을 입력한 후 Download Key Pair를 클릭합니다.
Key Pair를 PC의 임의 위치에 저장한 후 **\[Launch Instances\]** 를 클릭합니다. (인스턴스 기동에 몇 분이 소요될 수 있습니다.)
![aws-ec2-select-keypair](/analytics-on-aws/images/aws-ec2-select-keypair.png)
12. MacOS 사용자의 경우, 다운로드 받은 Key Pair 파일의 File Permission을 400으로 변경합니다.
    ```shell script
    $ chmod 400 ./analytics-hol.pem 
    $ ls -lat analytics-hol.pem 
    -r--------  1 ******  ******  1692 Jun 25 11:49 analytics-hol.pem
    ```
    Windows OS 사용자의 경우, [PuTTY를 사용하여 Windows에서 Linux 인스턴스에 연결](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)를 참고하십시요.
