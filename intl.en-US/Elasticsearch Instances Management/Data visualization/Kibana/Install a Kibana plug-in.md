---
keyword: [Kibana plug-in, install a Kibana plug-in]
---

# Install a Kibana plug-in

In addition to open source community plug-ins, Alibaba Cloud Kibana provides a variety of built-in plug-ins. This topic describes how to install a Kibana plug-in and provides the related precautions.

-   An Alibaba Cloud Elasticsearch cluster of a version earlier than V7.0 is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

    **Note:** Alibaba Cloud Elasticsearch clusters of V7.0 or later do not support Kibana plug-ins.

-   Your Kibana node offers at least 2 vCPUs and 4 GiB of memory. Plug-ins consume substantial resources.

    If the specifications of your Kibana node do not meet these requirements, upgrade the Kibana node. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Data Visualization**.

5.  In the **Kibana** section of the page that appears, click **Edit Configuration**.

6.  In the **Plug-in Configuration** section, find the plug-in that you want to install and click **Install** in the **Actions** column.

    If the specifications of your Kibana node do not meet the preceding requirements, the system prompts you to upgrade the configuration of your cluster. Follow the instructions to upgrade the Kibana node.

    **Warning:** After you confirm the installation of the plug-in, the system restarts the Kibana node. During the restart, Kibana cannot provide services. Therefore, before you confirm the installation, make sure that the restart does not affect your operations in the Kibana console.

7.  In the **Install Plug-in** message, click **OK**.

    Then, the system restarts the Kibana node. After the node is restarted, the plug-in is installed.

    After the plug-in is installed, the state of the plug-in changes to **Installed**.

    **Note:** If you no longer require an installed plug-in, you can remove it. To remove a plug-in, go to the **Plug-in Configuration** section, find the plug-in, and click **Remove** in the **Actions** column. After you confirm the removal of the plug-in, the system restarts the Kibana node. During the restart, Kibana cannot provide services. Therefore, before you confirm the removal, make sure that the restart does not affect your operations in the Kibana console.


