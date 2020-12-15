---
title: "Amazon Elasticsearch Service"
weight: 37
pre: "<b>3-7. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.svg)

## 실시간 데이터 분석을 위한 Amazon Elasticsearch Service 생성하기

실시간으로 데이터를 저장하고, 분석하기 위해서 Elasticsearch cluster를 생성합니다.
Amazon ES 도메인은 Elasticsearch 클러스터와 동의어입니다. 도메인은 설정, 인스턴스 유형, 인스턴스 수, 스토리지 리소스를 지정한 설정입니다.

1. AWS Management Console에서 Analytics의 **Elasticsearch** 서비스를 선택합니다.
2. (Step 1: Choose deployment type) **Create a new domain(새 도메인 생성)** 을 선택합니다.
3. **Elasticsearch 도메인 생성** 페이지에서 **Deployment type(배포 유형)** 에 대해 **Production(프로덕션)** 을 선택합니다.
4. **버전**에서 해당 도메인의 Elasticsearch 버전을 선택합니다. 지원되는 최신 버전을 선택하는 것이 좋습니다. 자세한 내용은 [지원되는 Elasticsearch 버전](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/what-is-amazon-elasticsearch-service.html#aes-choosing-version) 단원을 참조하십시오.
5. **\[Next\]** 를 선택합니다.
6. (Step 2: Configure domain) 도메인의 이름을 입력합니다. 이 실습에서는 이후에 다룰 `retail`를 예제 도메인 이름으로 사용합니다.
7. **인스턴스 유형** 에서 Amazon ES 도메인의 인스턴스 유형을 선택합니다. 이 실습에서는 테스트 목적에 적합한 소용량의 경제적인 인스턴스 유형
`t2.medium.elasticsearch`를 사용하는 것이 좋습니다.
8. **인스턴스 수** 에 원하는 인스턴스 수를 입력합니다. 이 실습에서는 기본값 `1`을 사용합니다.
9. 스토리지 유형에서 EBS를 선택합니다.
    + a. EBS volume type(EBS 볼륨 유형)에 일반용(SSD)을 선택합니다. 자세한 내용은 Amazon EBS 볼륨 유형을 참조하십시오.
    + b. EBS volume size(EBS 볼륨 크기)에 각 데이터 노드용 외부 스토리지의 크기를 GiB 단위로 입력합니다. 이 실습에서는 기본값 `10`을 사용합니다.
10. 지금은 **Dedicated master nodes(전용 마스터 노드), Snapshot configuration(스냅샷 구성)** 및 **Optional Elasticsearch cluster settings(선택적 Elasticsearch 클러스터 설정)** 섹션을 무시할 수 있습니다.
11. **\[Next\]** 를 선택합니다.
12. (Step 3: Configure access and security) **Network configuration(네트워크 구성)** 의 경우 **VPC access** 를 선택합니다.
적절한 VPC와 subnet을 선택합니다. Security Groups으로 준비 단계에서 생성한 `es-cluster-sg`를 선택합니다.
13. 지금은 **Amazon Cognito Authentication(Amazon Cognito 인증)** 과 **Fine–grained access control** 을 disable 합니다.
14. **Access policy(액세스 정책)** 의 경우 **Domain access policy(도메인 액세스 정책)** 에서 **JSON defined access policy(JSON 정의 액세스 정책)** 선택한 다음,
**Add or edit the access policy(액세스 정책 추가 또는 편집)** 에 다음 템플릿을 이용해서 **JSON defined access policy** 를 생성해서 입력 합니다.
    + JSON defined access policy Template - `<DOMAIN-NAME>` 에 **(Step 2: Configure domain)** 에서 입력한 도메인 이름을 일력합니다.
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
    + 예) 이번 실습에서는 `retail` 을 도메인 이름으로 사용했기 때문에, 아래와 같이 JSON defined access policy 를 생성합니다.
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
15. **Encryption(암호화)** 에서 **Require HTTPS for all traffic to the domain** 만 허용하고, 다른 항목은 disable 합니다.
16. **Encryption(암호화)** 의 모든 기본값을 유지합니다. **\[Next\]** 를 선택합니다.
17. **Review** 페이지에서 도메인 구성을 검토한 다음 **확인**을 선택합니다.

