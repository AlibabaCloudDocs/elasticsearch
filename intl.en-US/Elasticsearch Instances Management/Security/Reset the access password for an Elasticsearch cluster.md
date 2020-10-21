---
keyword: [reset the access password of your Elasticsearch cluster, reset the access password of the Kibana console for your Elasticsearch cluster]
---

# Reset the access password for an Elasticsearch cluster

This topic describes how to reset the password of the elastic user that is used to access your Elasticsearch cluster. After you reset the password, if you use the elastic user to log on to your Elasticsearch cluster and the Kibana console of the cluster, you must use the new password of the elastic user.

An Alibaba Cloud Elasticsearch cluster is created. For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md).

Note the following points when you reset the password:

-   This operation resets only the password of the elastic user. We recommend that you use a custom account that is granted the required permissions instead of the elastic account to log on to your Elasticsearch cluster.
-   After you confirm the operations, the system does not restart the Elasticsearch cluster.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the page that appears, click **Security**.

5.  In the **Network Settings** section of the page that appears, click **Reset** next to **Elasticsearch Cluster Password**.

6.  In the **Reset** pane, enter the new password for **elastic** and confirm the password.

    ![Reset the password](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8802523061/p40174.jpg)

7.  Click **OK**.

    After you reset the password, the new password takes effect in about five minutes.


