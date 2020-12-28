---
title: "QuickSight"
weight: 35
pre: "<b>3-5. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## QuickSight를 이용한 데이터 시각화

이번에는 Amazon QuickSight를 통해 데이터 시각화 작업을 합니다.

1. [QuickSight 콘솔](https://quicksight.aws.amazon.com)로 이동합니다.
2. QuickSight에 가입하기 위해 **\[Sign up for QuickSight\]** 버튼을 클릭합니다.
3. Standard Edition을 선택한 후 **\[Continue\]** 버튼을 클릭합니다.
4. QuickSight account name은 임의로 지정(중복될 경우 계정이 생성되지 않습니다) 하고,
Notification email address는 개인 Email 주소를 입력합니다.
    ![qs-account](/analytics-on-aws/images/qs-account.png)

5. QuckSight가 S3에 Access해야 하므로, **\[Choose S3 buckets\]** 를 클릭합니다.

    ![qs-select-s3](/analytics-on-aws/images/qs-select-s3.png)

6. 아래와 같은 창이 뜨면, 데이터가 저장되어 있는 `aws-analytics-immersion-day-xxxxxxxx` 버킷을 선택합니다. 
    ![qs-select-bucket](/analytics-on-aws/images/qs-select-bucket.png)

7. **\[Finish\]** 를 클릭합니다.
8. 계정이 생성된 후 **\[Go to Amazon QuickSight\]** 버튼을 클릭합니다.
9. 우측 상단의 사용자 아이콘을 클릭하여 에 region이 데이터를 저장하고 있는 S3 bucket의 region과 동일한지 확인합니다.


10. 우측 상단의 **\[New analysis\]** 버튼을 클릭합니다.
11. 좌측 상단 **\[New Data Set\]** 버튼을 클릭합니다.
![aws-quicksight-new_data_sets](/analytics-on-aws/images/aws-quicksight-new_data_sets.png)
8. `Athena` 를 클릭하고 팝업 창의 Data source name에 `retail-quicksight` 를 입력(임의의 값 입력 가능)하고,
**\[Validate connection\]** 을 클릭 해서 `Validated` 상태로 변경되면, **\[Create data source\]** 버튼을 클릭합니다.
![aws-quicksight-athena_data_source](/analytics-on-aws/images/aws-quicksight-athena_data_source.png)
9. Choose your table 화면에서 Database는 `mydatabase` (앞서 생성한 Athena 데이터베이스),
Tables 에서 `retail_trans_json` 를 선택하고 Select 버튼을 클릭합니다.
![aws-quicksight-athena-choose_your_table](/analytics-on-aws/images/aws-quicksight-athena-choose_your_table.png)
10. Finish data set creation 화면에서 **\[Visualize\]** 버튼을 클릭 합니다.
`retail_trans_json` 테이블 데이터가 QuickSight SPICE 엔진에 로딩 되었는지 확인합니다.
![qs-finish-dataset](/analytics-on-aws/images/qs-finish-dataset.png)
11. `InvoicdDate` 별 `Quantity`, `Price`를 시각화 해 보겠습니다. 좌측 Fields list에서 `invoicedate`, `price`, `quantity` field를 차례대로 선택합니다.
Visual types는 세로 막대 그래프를 선택합니다.
![aws-quicksight-bar-chart](/analytics-on-aws/images/aws-quicksight-bar-chart.png)
12. 방금 만든 Dashboard를 다른 사용자에게 공유해 보겠습니다. 우측 상단 유저 아이콘을 클릭하고 \[Manage QuickSight\] 를 클릭합니다.
![qs-manage-qs](/analytics-on-aws/images/qs-manage-qs.png)
13. **Invite users** 버튼을 클릭한 후 임의의 사용자 계정명(BI_user01)을 입력한 후 우측 **\[+\]** 버튼을 클릭합니다.
Email은 다른 사용자의 Email 주소를 입력하고 Role은 AUTHOR, IAM User는 NO를 선택한 후 Invite 버튼을 클릭합니다.
![aws-quicksight-user-invitation](/analytics-on-aws/images/aws-quicksight-user-invitation.png)
14. 사용자는 다음과 같은 Invitation Email을 받고 Click to accept invitation을 클릭하면 계정 생성 메뉴에서 비밀번호를 변경할 수 있습니다.
![aws-quicksight-user-email](/analytics-on-aws/images/aws-quicksight-user-email.png)
15. QuickSight 화면으로 돌아가서 우측 상단의 **Share > Share analysis** 를 클릭합니다.
![aws-quicksight-share-analysis.png](/analytics-on-aws/images/aws-quicksight-share-analysis.png)
16. BI_user01을 선택한 후 Share 버튼을 클릭합니다.
![aws-quicksight-share-analysis-users](/analytics-on-aws/images/aws-quicksight-share-analysis-users.png)
17. 사용자는 다음과 같은 Email을 수신합니다. **\[Click to View\]** 를 클릭하여 분석결과를 확인할 수 있습니다.
![aws-quicksight-user-email-click-to-view](/analytics-on-aws/images/aws-quicksight-user-email-click-to-view.png)
