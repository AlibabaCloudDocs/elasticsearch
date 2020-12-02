---
keyword: Logstash clusters
---

# Clusters

The Logstash Clusters page provides the information of the created Logstash clusters. You can also refresh the status of the clusters, manage pipelines, and manage clusters on this page.

Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/logstashes). The **Clusters** page appears. The **Clusters** page displays all Logstash clusters in the selected region under your Alibaba Cloud account.

The following table describes the operations that you can perform on this page.

|Operation|Description|
|---------|-----------|
|View the information of clusters|You can view the following information: **Cluster ID/Name**, **Status**, **Version**, **Data Nodes**, **Cluster Specification**, **Zone**, **Billing Method**, **Network Type**, and **Created At**.|
|View details about a cluster|Click the ID of the cluster that you want to view in the **Cluster ID/Name** column. On the page that appears, view details about the cluster in the **Basic Information** section.|
|Create a Logstash cluster|Click **Create**. On the buy page, create a cluster. For more information, see [Create a Logstash cluster]().|
|Refresh the status of clusters|Click **Refresh** to view the real-time status of clusters. After a cluster is created, it is in the **Initializing** state. You can click **Refresh** to refresh the status of the cluster. After **Status** of the cluster becomes **Active**, you can use the cluster.|
|Manage pipelines|Click **Manage Pipelines** in the **Actions** column of a cluster. On the **Pipelines** page, create and configure pipelines. For more information, see [Manege pipelines]().|
|Manage a cluster|Click **Manage Clusters** in the **Actions** column of a cluster. On the page that appears, manage the cluster, such as viewing details about the cluster, configuring the cluster, configuring plug-ins, configuring cluster monitoring, querying logs, and managing pipelines.|
|Switch the billing method from pay-as-you-go to subscription|This feature only applies to pay-as-you-go clusters. Choose **More** \> **Switch to Subscription** in the **Actions** column of a cluster. On the **Confirm Order** page, switch the billing method.|
|Modify the configurations of a cluster|Choose **More** \> **Update Configuration** in the **Actions** column of a cluster. On the **Upgrade/Downgrade** page, modify the configurations of the cluster.|
|Release a cluster|Choose **More** \> **Release** in the **Actions** column of a cluster. In the **Release** message that appears, click OK to release the cluster. **Warning:** After a cluster is released, all the data in the cluster is lost and cannot be recovered. Exercise caution when you release clusters. |

