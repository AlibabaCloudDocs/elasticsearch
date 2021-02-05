# Step 2: \(Optional\) Configure a cluster

After you create an Alibaba Cloud Elasticsearch cluster, you can configure the cluster to improve search efficiency and service security.

## Enable the Auto Indexing feature

**Note:**

-   The **Auto Indexing** feature is designed for testing purposes. We recommend that you do not enable this feature in production environments.
-   Before you import data into an Elasticsearch cluster, you must manually create both indexes and mappings for the data. If you do not create both indexes and mappings, errors may occur. For example, if you delete the mappings for a document and then upload another document that does not have mappings, the Auto Indexing feature creates mappings for both documents. The created mappings may not be suitable for your application. To avoid this issue, Elasticsearch disables the **Auto Indexing** feature by default.
-   After you enable the **Auto Indexing** feature, the system restarts your Elasticsearch cluster. Therefore, before you enable this feature, make sure that the restart does not affect your services.

If you must use the **Auto Indexing** feature, perform the following steps to enable this feature:

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  On the **Cluster Configuration** page, click **Modify Configuration** on the right side of **YML Configuration**.

6.  In the **YML File Configuration** pane, set **Auto Indexing** to **Enable**.

7.  Select **This operation will restart the cluster. Continue?** and click **OK**.

    The system then restarts the cluster. After the cluster is restarted, the **Auto Indexing** feature is enabled.


## Configure plug-ins

On the Plug-ins page, you can install or remove built-in plug-ins, use the standard or rolling update method to update IK dictionaries, or upload custom plug-ins. For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md), [Use the analysis-ik plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-ik plug-in.md), and [Upload and install a custom plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Upload and install a custom plug-in.md).

## Configure security settings

On the Security page, you can reset the password that is used to access your Elasticsearch cluster, configure a virtual private cloud \(VPC\) whitelist, and enable the Public Network Access feature. You can also configure a public IP address whitelist, enable HTTPS, or configure cluster interconnection. For more information, see [Security](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).

## Configure monitoring and alerting settings

Alibaba Cloud Elasticsearch can monitor the following metrics of Elasticsearch clusters and send text messages to alert users. You can customize alerting thresholds for the metrics. For more information, see [Configure Elasticsearch alerts in Cloud Monitor](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Configure the monitoring and alerting feature in Cloud Monitor.md).

-   ClusterStatus
-   ClusterQueryQPS\(Count/Second\)
-   ClusterIndexQPS\(Count/Second\)
-   NodeCPUUtilization\(%\)
-   NodeDiskUtilization\(%\)
-   NodeHeapMemoryUtilization\(%\)
-   NodeLoad\_1m

