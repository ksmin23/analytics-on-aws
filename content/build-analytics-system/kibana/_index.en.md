---
title: "Kibana"
weight: 39
pre: "<b>3-9. </b>"
---

![aws-analytics-system-build-steps](/analytics-on-aws/images/aws-analytics-system-build-steps.svg)

## Data visualization with Kibana

Visualize data collected from Amazon Elasticsearch Service using Kibana.

1. To access the Elasticsearch Cluster, add the ssh tunnel configuration to the ssh config file of the personal local PC as follows.
    ```shell script
    # Elasticsearch Tunnel
    Host estunnel
      HostName <EC2 Public IP of Bastion Host>
      User ec2-user
      IdentitiesOnly yes
      IdentityFile ~/.ssh/analytics-hol.pem
      LocalForward 9200 <Elasticsearch Endpoint>:443
    ```
  + **EC2 Public IP of Bastion Host** uses the public IP of the EC2 instance created in the **Lab setup** step.
  + ex)
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
2. Run `ssh -N estunnel` in Terminal.
3. Connect to `https://localhost:9200/_plugin/kibana/` in a web browser.
4. (Home) Click **\[Use Elasticsearch data / Connect to your Elasticsearch index\]** in **Add Data to Kibana**.
![kibana-01-add_data](/analytics-on-aws/images/kibana-01-add_data.png)
5. (Management / Create index pattern) In **Step 1 of 2: Define index pattern** of **Create index pattern**, enter `retail*` in Index pattern.
![kibana-02a-create-index-pattern](/analytics-on-aws/images/kibana-02a-create-index-pattern.png)
6. (Management / Create index pattern) Choose **\[> Next step\]**.
7. (Management / Create index pattern) Select `InvoiceDate` for the Time Filter field name in **Step 2 of 2: Configure settings** of the Create index pattern.
![kibana-02b-create-index-pattern-configure-settings](/analytics-on-aws/images/kibana-02b-create-index-pattern-configure-settings.png)
8. (Management / Create index pattern) Click **\[Create index pattern\]**.
![kibana-02c-create-index-pattern-review](/analytics-on-aws/images/kibana-02c-create-index-pattern-review.png)
9. (Management / Advanced Settings) After selecting **\[Advanced Settings\]** from the left sidebar menu, set **Timezone for date formatting** to `Etc/UTC`. Since the log creation time of the test data is based on `UTC`, **Kibana**'s **Timezone** is also set to `UTC`.
![kibana-02d-management-advanced-setting](/analytics-on-aws/images/kibana-02d-management-advanced-setting.png)
10. (Discover) After completing the creation of **Index pattern**, select **Discover** to check the data collected in Elasticsearch.
![kibana-03-discover](/analytics-on-aws/images/kibana-03-discover.png)
11.  (Discover) Let's visualize the `Quantity` by `InvoicdDate`. Select **invoicdDate** from **Available fields** on the left, and click **Visualize** at the bottom.
![kibana-04-discover-visualize](/analytics-on-aws/images/kibana-04-discover-visualize.png)
12. (Visualize) After selecting **Y-Axis** in **Metrics** on the Data tab, apply `Sum` for **Aggregation**, and `Quantity` for **Field** as shown below.
![kibana-05-discover-change-metrics](/analytics-on-aws/images/kibana-05-discover-change-metrics.png)
13. (Visualize) Click **\[Save\]** in the upper left corner, write down the name of the graph you saved, and then click **\[Confirm Save\]**.
![kibna-08-visualize-save](/analytics-on-aws/images/kibana-08-visualize-save.png)
14. (Dashboards) Click **Dashboard** icon on the left and click the **\[Create new dashboard\]** button.
![kibana-09-dashboards](/analytics-on-aws/images/kibana-09-dashboards.png)
15. (Dashboards) Click **\[Add\]** on the upper left, and select the graph created in the previous step in **Add Panels**.
![kibana-10-import-visualization](/analytics-on-aws/images/kibana-10-import-visualization.png)
16. (Dashboards) Click **\[Save\]** at the top left, enter **Title** in the **Save dashboard**, and click **\[Confirm Save\]**.
![kibana-12-discover-save-dashboard](/analytics-on-aws/images/kibana-12-discover-save-dashboard.png)
17. (Dashboards) You can see the following Dashboards.
![kibana-13-complete](/analytics-on-aws/images/kibana-13-complete.png)
