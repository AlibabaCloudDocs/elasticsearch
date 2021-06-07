# Enable the X-Pack Monitoring feature

This topic describes how to enable the X-Pack Monitoring feature to monitor your Alibaba Cloud Logstash cluster. After you enable the feature for your Logstash cluster and associate the cluster with an Elasticsearch cluster, you can monitor the Logstash cluster in the Kibana console of the Elasticsearch cluster.

-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).

-   An Alibaba Cloud Elasticsearch cluster is created. Make sure that the Elasticsearch and Logstash clusters reside in the same virtual private cloud \(VPC\) and use the same version.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).


1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the **Logstash Clusters** page, find the desired cluster and click its ID.

4.  In the left-side navigation pane of the page that appears, click **Cluster Monitoring**.

5.  In the **Monitoring and Alerting** section, click **Edit** on the right side of **X-Pack Monitoring**.

    ![Enable X-Pack Monitoring](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67546.png)

6.  In the **Edit Configuration** panel, set **X-Pack Monitoring** to Enable and associate the Logstash cluster with your Elasticsearch cluster.

    ![Edit Configuration panel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67560.png)

    |Parameter|Description|
    |---------|-----------|
    |**Elasticsearch Cluster**|Select the Elasticsearch cluster with which you want to associate the Logstash cluster. We recommend that you select an Elasticsearch cluster that is in the same VPC and of the same version as the Logstash cluster. If the Elasticsearch and Logstash clusters are of different versions, make sure that the versions of the clusters are compatible with each other.|
    |**Username**|Enter the username of the Elasticsearch cluster.|
    |**Password**|Enter the password of the Elasticsearch cluster.|

7.  Click **Test Connectivity**.

    If no error is reported, the Elasticsearch cluster is connected.

    **Warning:** Enabling or disabling X-Pack Monitoring triggers a restart of the Logstash cluster. This may affect your services. Proceed with caution.

8.  Click **OK**.

    The system returns to the **Cluster Monitoring** page and restarts the Logstash cluster.

9.  After the Logstash cluster is restarted, view the monitoring data of the cluster.

    After the Logstash cluster is restarted, the value of **X-Pack Monitoring** changes to **Enable**, and the associated Elasticsearch cluster appears.

    **Note:** You can view the monitoring data of the Logstash cluster in the Kibana console only after the cluster is restarted.

    1.  On the **Cluster Monitoring** page, click **Kibana Console**.

        ![X-Pack Monitoring enabled](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67571.png)

    2.  Log on to the Kibana console of the associated Elasticsearch cluster.

        For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

    3.  In the left-side navigation pane, click **Monitoring**.

    4.  In the **Logstash** section, view the monitoring data of the Logstash cluster.

        ![Logstash monitoring page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67575.png)


