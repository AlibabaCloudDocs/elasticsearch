---
keyword: install a Logstash plug-in
---

# Install a Logstash plug-in

Before you can use a Logstash plug-in, you must install the plug-in. This topic describes how to install a plug-in of Alibaba Cloud Logstash.

An Alibaba Cloud Logstash cluster is created. For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Plug-ins**.

5.  On the **Built-in Plug-ins** tab, find the target plug-in and click **Install** in the Actions column.

    ![Install a Logstash plug-in](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2838986061/p95673.png)

    **Warning:** The installation of a plug-in triggers a restart of the Alibaba Cloud Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

6.  In the **Install Plug-in** message, carefully read the system notification and click **OK**.

    The restart of the Alibaba Cloud Logstash cluster is in progress. During the restart process, you can view the task progress in the Tasks dialog box. For more information, see [View progress of running cluster tasks](/intl.en-US/Logstash/Cluster management/View progress of running cluster tasks.md). After the cluster is restarted, the plug-in is installed.

    **Note:** If you no longer need the plug-in, find it and click **Remove** in the Actions column to remove the plug-in. The removal of the plug-in also triggers a restart of the Alibaba Cloud Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.


