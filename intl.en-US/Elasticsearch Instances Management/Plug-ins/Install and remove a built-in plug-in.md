---
keyword: [Alibaba Cloud Elasticsearch default plug-in, Alibaba Cloud Elasticsearch built-in plug-in]
---

# Install and remove a built-in plug-in

After you purchase an Alibaba Cloud Elasticsearch cluster, built-in plug-ins are displayed on the Built-in Plug-ins tab. You can install or remove these plug-ins based on your business requirements. This topic describes how to install and remove a built-in plug-in for Alibaba Cloud Elasticsearch.

For more information about the plug-ins that are supported by Alibaba Cloud Elasticsearch, Elasticsearch versions in which the plug-ins are available, and the operations that are supported by the plug-ins, see [Overview](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Overview.md). The **analysis-ik** and **elasticsearch-repository-oss** plug-ins are both extensions of Alibaba Cloud Elasticsearch. You cannot remove these plug-ins.

-   **analysis-ik**: an IK analysis plug-in. In addition to its open source features, this plug-in supports the dynamic loading of dictionaries stored in Object Storage Service \(OSS\). You can update dictionaries by using the standard update or rolling update method. For more information, see [Use the analysis-ik plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-ik plug-in.md).
-   **elasticsearch-repository-oss**: In addition to its open source features, this plug-in provides OSS storage while you create and restore index snapshots.

## Precautions

If you install or remove a built-in plug-in, the cluster is restarted. Alibaba Cloud Elasticsearch removes only the plug-in that you select. Before you proceed, confirm the operation.

## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane, click **Plug-ins**.

5.  On the **Built-in Plug-ins** tab, find your desired plug-in and click **Install** or **Remove** in the **Actions** column.

6.  Read the message that appears and click **OK**.

    Then, the system restarts the cluster. During the restart, you can view the task progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box.


## References

-   [ListPlugins](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/ListPlugins.md)
-   [InstallSystemPlugin](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/InstallSystemPlugin.md)
-   [UninstallPlugin](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/UninstallPlugin.md)

