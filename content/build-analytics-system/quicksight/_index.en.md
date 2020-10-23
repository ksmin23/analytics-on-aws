---
title: "QuickSight"
weight: 35
pre: "<b>3-5. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.png)

## Data visualization with QuickSight

At this time, let's use Amazon QuickSight to visualize the data.

1. Go to [QuickSight Console](https://quicksight.aws.amazon.com).
2. Click the **\[Sign up for QuickSight\]** button to sign up for QuickSight.
3. Select Standard Edition and click the **\[Continue\]** button.
4. Specify the QuickSight account name randomly (in case of duplicate, the account will not be created), and enter your personal email address for Notification email address.
QuckSight needs to access S3 so that click **\[Choose S3 buckets\]**. Select `aws-analytics-immersion-day-xxxxxxxx` and click **\[Finish\]**.
5. After the account is created, click the **\[Go to Amazon QuickSight\]** button.
6. After setting the **region** in the upper right corner as the region of the S3 bucket storing data, click **\[New Analysis\]** in the upper left corner.
7. Click **\[New Data Set\]** button.
![aws-quicksight-new_data_sets](/analytics-on-aws/images/aws-quicksight-new_data_sets.png)
8. Click `Athena` and enter `retail-quicksight` in the Data source name in the pop-up window (any value can be entered).
Click **\[Validate connection\]** to change to `Validated`, then click the **\[Create data source\]** button.
![aws-quicksight-athena_data_source](/analytics-on-aws/images/aws-quicksight-athena_data_source.png)
9. In the Choose your table screen, Database is `mydatabase` (the Athena database created earlier), and then Select `retail_trans_json` from Tables and click the Select button.
![aws-quicksight-athena-choose_your_table](/analytics-on-aws/images/aws-quicksight-athena-choose_your_table.png)
10. Click the **\[Visualize\]** button on the Finish data set creation screen.
Check if the `retail_trans_json` table data is loaded into the QuickSight SPICE engine.
11. Let's visualize the `Quantity` and `Price` by ʻInvoicdDate`. Select the ʻinvoicedate`, `price`, and `quantity` fields in order from the left Fields list.
Select vertical bar graph as Visual types.
![aws-quicksight-bar-chart](/analytics-on-aws/images/aws-quicksight-bar-chart.png)
12. Let's share the Dashboard we just created with other users. Click the user icon in the upper left corner and click \[Manage QuickSight\].
13. Click the Invite users button, enter a random user account name (BI_user01), and click the **\[+\]** button on the right.
Enter the email address of another user for Email, select AUTHOR for Role, and NO for IAM User, and click the Invite button.
![aws-quicksight-user-invitation](/analytics-on-aws/images/aws-quicksight-user-invitation.png)
14. Users receive the following Invitation Email and click Click to accept invitation to change their password in the Create Account menu.
![aws-quicksight-user-email](/analytics-on-aws/images/aws-quicksight-user-email.png)
15. Return to the QuickSight screen and click **Share> Share analysis** in the upper right.
![aws-quicksight-share-analysis.png](/analytics-on-aws/images/aws-quicksight-share-analysis.png)
16. Select BI_user01 and click the Share button.
![aws-quicksight-share-analysis-users](/analytics-on-aws/images/aws-quicksight-share-analysis-users.png)
17. Users receive the following email: You can check the analysis results by clicking **\[Click to View\]**.
![aws-quicksight-user-email-click-to-view](/analytics-on-aws/images/aws-quicksight-user-email-click-to-view.png)
