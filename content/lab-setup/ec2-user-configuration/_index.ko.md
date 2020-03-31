---
title: "EC2 설정"
weight: 24
pre: "<b>2-4 </b>"
tag:
  - EC2_Configuration
---

생성한 EC2 인스턴스가 다른 AWS 리소스에 접근 및 제어할 수 있도록 다음과 같이 구성합니다.
1. 생성한 인스턴스의 Public IP로 SSH 접속을 합니다.
    ```shell script
    ssh -i "<Key pair name>" ec2-user@<Public IP>
    ```
2. ssh로 접속한 EC2 인스턴스에서 다음 작업을 순서대로 수행 합니다.

    (1) 소소 코드를 다운로드 받는다. 
    ```shell script
    wget 'https://github.com/ksmin23/aws-analytics-immersion-day/archive/master.zip'
    ```
    (2) 다운로드 받은 소스 코드의 압축을 해제한다.
    ```shell script
    unzip -u master.zip
    ```
    (3) 실습 환경 설정 스크립트에 실행 권한을 부여한다.
    ```shell script
    chmod +x ./aws-analytics-immersion-day-master/set-up-hands-on-lab.sh
    ```
    (4) 실습 환경 설정 스크립트를 실행한다.
    ```shell script
    ./aws-analytics-immersion-day-master/set-up-hands-on-lab.sh
    ```
    (5) 실습 환경 설정 스크립트 실행 후, 실습에 필요한 파일들이 정상적으로 생성되었는지 확인한다. 
    예를 들어 아래와 같이 소스 코드와 필요한 파일들이 존재하는지 확인하다.
    
    ![aws-ec2-setup-hands-on-lab](/analytics-on-aws/images/aws-ec2-setup-hands-on-lab.png)

3. AWS의 다른 리소스 접근을 위해 AWS Configure를 진행합니다. 이때 앞서 생성한 IAM User 데이터를 활용합니다.
이전에 다운로드 받은 .csv 파일을 열어 `Access key ID`와 `Secret access key`를 확인하고 입력합니다.
    ```shell script
    $ aws configure
    AWS Access Key ID [None]: <Access key ID>
    AWS Secret Access Key [None]: <Secret access key>
    Default region name [None]: us-west-2
    Default output format [None]: 
    ```
4. 설정이 완료 되었다면 다음과 같이 입력하신 정보가 마스킹 되어 보이게 됩니다.
    ```shell script
    $ aws configure
    AWS Access Key ID [****************EETA]:
    AWS Secret Access Key [****************CixY]:
    Default region name [None]: us-west-2
    Default output format [None]: 
    ```

