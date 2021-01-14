---
title: "EC2 설정"
weight: 24
pre: "<b>2-3 </b>"
tag:
  - EC2_Configuration
---

생성한 EC2 인스턴스가 다른 AWS 리소스에 접근 및 제어할 수 있도록 다음과 같이 구성합니다.

## EC2가 사용할 IAM User 설정
AWS의 다른 리소스 접근을 위해 인스턴스가 사용할 IAM Role 을 생성합니다. IAM Role은 특정 권한을 가진 IAM 자격 증명입니다. IAM 역할의 경우, IAM 사용자 및 AWS가 제공하는 서비스에 사용할 수 있습니다. 서비스에 IAM Role을 부여할 경우, 서비스가 사용자를 대신하여 수임받은 역할을 수행합니다. 

1. [여기](https://console.aws.amazon.com/iam/home#/roles$new?step=type&commonUseCase=EC2%2BEC2&selectedUseCase=EC2&policies=arn:aws:iam::aws:policy%2FAdministratorAccess)를 클릭하여 IAM Role 페이지에 접속합니다.
2. **AWS service** 및 **EC2**가 선택된 것을 확인하고 **Next: Permissions**를 클릭합니다.
3. **AdministratorAccess** 정책이 선택된 것을 확인하고 **Next: Tags**를 클릭합니다.
4. 태그 추가(선택 사항) 단계에서 **Next: Review**를 클릭합니다.
5. **Role name**에 `analytics-admin`을 입력한 후, AdministratorAccess 관리형 정책이 추가된 것을 확인하고 **Create role**을 클릭합니다.
![AWS IAM Role](/analytics-on-aws/images/iam-role.png)

1. 그 다음 [여기](https://console.aws.amazon.com/ec2/v2/home#Instances:instanceState=running)를 클릭하여 EC2 인스턴스 페이지에 접속합니다.
2. 해당 인스턴스를 선택 후, **Actions > Security > Modify IAM Role**을 클릭합니다.
![EC2 Modify IAM Role](/analytics-on-aws/images/ec2-modify-iam-role.png)
1. IAM Role에서 `analytics-admin`을 선택한 후, **Apply** 버튼을 클릭합니다.
![EC2 Attach Role](/analytics-on-aws/images/ec2-role.png)

## EC2에 실습에 필요한 파일 설정
1. 생성된 EC2 인스턴스에 접근하는 데에는 세 가지 방법이 있습니다. 

    (1) EC2 인스턴스 연결 브라우저 기반 클라이언트인 EC2 Instance Connect를 사용하여 연결합니다.

    (2) AWS Systems Manager 의 Session Manager 기능을 사용해서 연결합니다. SSH 키 또는 배스천 호스트 없이 인스턴스에 연결하며, 연결 세션은 AWS Key Management Service 키를 사용하여 보호됩니다. 세션 명령 및 세부 정보를 S3 버킷이나 Cloudwatch Logs 로그 그룹에 기록할 수 있습니다.

    (3) SSH 자체 클라이언트를 통해 접근합니다.

    아래에는 EC2 Instance Connect 를 사용하는 방법을 설명합니다. SSH 클라이언트를 통해 접근하고자 하시면 [이 설명](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)에 따라주십시오.
2. [EC2 콘솔](https://console.aws.amazon.com/ec2/v2/home#Instances:instanceState=running)에서 방금 생성한 인스턴스를 선택하고 **Connect** 를 클릭합니다.
    ![EC2 Connect](/analytics-on-aws/images/ec2-connect.png)

3. 아래와 같은 화면이 뜨면 Connect 를 눌러 인스턴스에 접속합니다. 

    ![EC2 SSM Connect](/analytics-on-aws/images/ec2-ssm.png)

4. ssh로 접속한 EC2 인스턴스에서 다음 작업을 순서대로 수행 합니다.

    (1) 소소 코드를 다운로드 받는다. 
    ```shell script
    wget 'https://github.com/ksmin23/aws-analytics-immersion-day/archive/main.zip'
    ```
    (2) 다운로드 받은 소스 코드의 압축을 해제한다.
    ```shell script
    unzip -u main.zip
    ```
    (3) 실습 환경 설정 스크립트에 실행 권한을 부여한다.
    ```shell script
    chmod +x ./aws-analytics-immersion-day-main/set-up-hands-on-lab.sh
    ```
    (4) 실습 환경 설정 스크립트를 실행한다.
    ```shell script
    ./aws-analytics-immersion-day-main/set-up-hands-on-lab.sh
    ```
    (5) 실습 환경 설정 스크립트 실행 후, 실습에 필요한 파일들이 정상적으로 생성되었는지 확인한다. 
    예를 들어 아래와 같이 소스 코드와 필요한 파일들이 존재하는지 확인하다.
    
    ![aws-ec2-setup-hands-on-lab](/analytics-on-aws/images/aws-ec2-setup-hands-on-lab.png)

