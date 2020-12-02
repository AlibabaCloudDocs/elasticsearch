# Enable X-Pack Monitoring

This topic describes how to enable X-Pack Monitoring to monitor an Alibaba Cloud Logstash cluster. After you enable X-Pack Monitoring for a Logstash cluster and associate the Logstash cluster with an Elasticsearch cluster, you can monitor the Logstash cluster in the Kibana console of the associated Elasticsearch cluster.

You have completed the following operations:

-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).

-   An Alibaba Cloud Elasticsearch cluster is created in the same Virtual Private Cloud \(VPC\) and version as the Logstash cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Cluster Monitoring**.

5.  In the **Monitoring and Alerting** section, click **Edit** next to **X-Pack Monitoring**.

    ![Enable X-Pack Monitoring](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67546.png)

6.  In the **Edit Configuration** pane, select Enable for **X-Pack Monitoring** and select the Elasticsearch cluster that you want to associate.

    ![Edit Configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67560.png)

    |Parameter|Description|
    |---------|-----------|
    |**Elasticsearch Cluster**|Select the Elasticsearch cluster that you use to associate with the target Logstash cluster. You can only select an Elasticsearch cluster in the same VPC and version as the target Logstash cluster.|
    |**Username**|Enter the username of the Elasticsearch cluster.|
    |**Password**|Enter the password of the username for the Elasticsearch cluster.|

7.  Click **Test Connectivity**.

    If no error is reported, the Elasticsearch cluster is connected.

    **Warning:** Enabling or disabling X-Pack Monitoring triggers a restart of the Logstash cluster. Before you can continue, make sure that the restart does not affect your services.

8.  Click **OK**.

    The system returns to the **Cluster Monitoring** page and triggers the cluster restart. After the cluster is restarted, X-Pack Monitoring is enabled and the Logstash cluster is associated with the Elasticsearch cluster. The status of **X-Pack Monitoring** changes to **Enable**, and the associated Elasticsearch cluster also appears.

    ![X-Pack Monitoring is enabled](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67571.png)

9.  View the monitoring information of the Logstash cluster.

    1.  On the **Cluster Monitoring** page, click **Kibana Console**.

    2.  Log on to the Kibana console. In the left-side navigation pane, click **Monitoring**. In the **Logstash** section, view the monitoring information of the Logstash cluster.

        ![Logstash monitoring page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7848986061/p67575.png)

        **Note:** For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).


