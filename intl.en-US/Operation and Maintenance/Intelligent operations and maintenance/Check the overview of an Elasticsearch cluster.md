---
keyword: Elasticsearch cluster status
---

# Check the overview of an Elasticsearch cluster

This topic describes how to use the Cluster Overview feature to check the health status of your Alibaba Cloud Elasticsearch cluster.

-   Intelligent Maintenance is enabled. For more information, see [Enable Intelligent Maintenance](/intl.en-US/Operation and Maintenance/Intelligent operations and maintenance/Enable Intelligent Maintenance.md).
-   At least one diagnostic is performed on your Elasticsearch cluster, and a diagnostic report is generated. For more information, see [Perform a diagnostic on an Elasticsearch cluster]().

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the cluster details page, click **Intelligent Maintenance**. Then, click **Cluster Overview**.

5.  On the **Cluster Overview** page, view the health status of your Elasticsearch cluster.

    The **Cluster Overview** page shows the health status of your Elasticsearch cluster in the last seven days. You can view this page to obtain information about the health status of your cluster.

    Intelligent Maintenance uses red, yellow, and green to indicate the health status of an Elasticsearch cluster.

    -   **Red**: A severe issue or threat that may affect the usage of the cluster exists and requires immediate processing. If you do not address this issue, data loss or cluster crash may occur.
    -   **Yellow**: A moderate issue or threat that may affect the usage of the cluster exists and requires prompt processing.
    -   **Green**: The cluster is healthy.

