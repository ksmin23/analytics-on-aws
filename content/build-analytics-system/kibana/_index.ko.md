---
title: "Kibana"
weight: 39
pre: "<b>3-9. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## Kibana를 이용한 데이터 시각화

Amazon Elasticsearch Service에서 수집된 데이터를 Kibana를 이용해서 시각화 작업을 합니다.

1. Elasticsearch Cluster에 접속하기 위해서 개인 Local PC의 ssh config 파일에 아래와 같이 ssh tunnel 설정을 추가 한다.
    ```shell script
    # Elasticsearch Tunnel
    Host estunnel
      HostName <EC2 Public IP of Bastion Host>
      User ec2-user
      IdentitiesOnly yes
      IdentityFile ~/.ssh/analytics-hol.pem
      LocalForward 9200 <Elasticsearch Endpoint>:443
    ```
  + **EC2 Public IP of Bastion Host** 은 **실습 환경 구성** 단계에서 생성한 EC2 인스턴스의 Public IP 를 사용한다.
  + 예)
    ```shell script
    ~$ ls -1 .ssh/
    analytics-hol.pem
    config
    id_rsa
    ~$ tail .ssh/config
    # Elasticsearch Tunnel
    Host estunnel
      HostName 214.132.71.219
      User ubuntu
      IdentitiesOnly yes
      IdentityFile ~/.ssh/analytics-hol.pem
      LocalForward 9200 vpc-retail-qvwlxanar255vswqna37p2l2cy.us-west-2.es.amazonaws.com:443
    ~$
    ```
2. Terminal 에서 `ssh -N estunnel` 를 실행합니다.
3. Web browser에서 `https://localhost:9200/_plugin/kibana/` 으로 접속합니다.
4. (Home) Add Data to Kibana 에서 **\[Use Elasticsearch data / Connect to your Elasticsearch index\]** 클릭한다.
![kibana-01-add_data](/analytics-on-aws/images/kibana-01-add_data.png)
5. (Management / Create index pattern) Create index pattern의 **Step 1 of 2: Define index pattern** 에서
Index pattern에 `retail*` 을 입력합니다.
![kibana-02a-create-index-pattern](/analytics-on-aws/images/kibana-02a-create-index-pattern.png)
6. (Management / Create index pattern) **\[> Next step\]** 을 선택합니다.
7. (Management / Create index pattern) Create index pattern의 **Step 2 of 2: Configure settings** 에서
Time Filter field name에 `InvoiceDate` 를 선택합니다.
![kibana-02b-create-index-pattern-configure-settings](/analytics-on-aws/images/kibana-02b-create-index-pattern-configure-settings.png)
8. (Management / Create index pattern) **\[Create index pattern\]** 을 클릭합니다.
![kibana-02c-create-index-pattern-review](/analytics-on-aws/images/kibana-02c-create-index-pattern-review.png)
9. (Management / Advanced Settings) 왼쪽 사이드바 메뉴에서 **\[Advanced Settings\]** 를 선택한 후, **Timezone for date formatting**을 `Etc/UTC` 로 설정합니다. 테스트용 데이터의 로그 생성 시간이 `UTC` 기준이기 때문에 Kibana의 Timezone 역시 `UTC`로 설정합니다.
![kibana-02d-management-advanced-setting](/analytics-on-aws/images/kibana-02d-management-advanced-setting.png)
10. (Discover) Index pattern 생성을 완료 후, Discover를 선택해서 Elasticsearch 수집된 데이터를 확인합니다.
![kibana-03-discover](/analytics-on-aws/images/kibana-03-discover.png)
11. (Discover) `InvoicdDate` 별 `Quantity`를 시각화 해 보겠습니다. 좌측의 Available fields에서 invoicdDate를
선택하고, 하단에 있는 Visualize를 클릭합니다.
![kibana-04-discover-visualize](/analytics-on-aws/images/kibana-04-discover-visualize.png)
12. (Visualize) 아래와 같이 Data 탭의 Metrics에서 Y-Axis를 Aggregation은 `Sum`, Field는 `Quantity`를 선택 후 적용 합니다.
![kibana-05-discover-change-metrics](/analytics-on-aws/images/kibana-05-discover-change-metrics.png)
13. (Visualize) 좌측 상단의 **\[Save\]** 를 클릭하고, 저장한 그래프의 이름을 적은 후에 **\[Confirm Save\]** 를 클릭합니다.
![kibna-08-visualize-save](/analytics-on-aws/images/kibana-08-visualize-save.png)
14. (Dashboards) 좌측의 Dashboard 아이콘을 클릭 후, **\[Create new dashboard\]** 버튼을 클릭 합니다.
![kibana-09-dashboards](/analytics-on-aws/images/kibana-09-dashboards.png)
15. (Dashboards) 좌측 상단의 **\[Add\]** 를 클릭해서, **Add Panels** 에 이전 단계에서 생성한 그래프를 선택 합니다.
![kibana-10-import-visualization](/analytics-on-aws/images/kibana-10-import-visualization.png)
16. (Dashboards) 좌측 상단의 **\[Save\]** 를 클릭 한 후, Save dashboard에서 Title을 입력한 이후, **\[Confirm Save\]** 를 클릭 합니다.
![kibana-12-discover-save-dashboard](/analytics-on-aws/images/kibana-12-discover-save-dashboard.png)
17. (Dashboards) 아래와 같은 Dashboard를 확인할 수 있습니다.
![kibana-13-complete](/analytics-on-aws/images/kibana-13-complete.png)

