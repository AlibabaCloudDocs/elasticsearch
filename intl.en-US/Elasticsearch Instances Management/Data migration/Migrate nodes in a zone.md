---
keyword: migrate Elasticsearch nodes
---

# Migrate nodes in a zone

Before you upgrade the configuration of your Elasticsearch cluster, you can migrate the nodes in the zone where the cluster resides to another zone. This ensures that the zone where the cluster resides provides sufficient resources for the upgrade. This topic describes how to migrate the nodes in a zone.

## Prerequisites

-   A zone with sufficient resources exists within the current account.

    We recommend that you select new zones from bottom to top in alphabetical order because these zones may have sufficient resources. For example, if both **cn-hangzhou-e** and **cn-hangzhou-h** are available, select **cn-hangzhou-h**. After you migrate nodes to another zone, you must manually update the configuration of your Elasticsearch cluster. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

-   Your Elasticsearch cluster is in the Active state.

    You can run the `GET _cat/health?v` command to check the status of the cluster.

-   [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) to check whether your cluster stores indexes in the close state. If your cluster stores such indexes, you must open or delete the indexes. Otherwise, the upgrade fails.
    -   Run the following command to view the statuses of indexes:

        ```
        GET /_cat/indices?v
        ```

        ![View the statuses of indexes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5135141261/p244657.png)

    -   Run the following command to enable the index in the close state:

        ```
        POST /<index_name>/_open
        ```

    -   Run the following command to delete the index in the close state:

        **Warning:** Proceed with caution because the deleted indexes cannot be recovered.

        ```
        DELETE /<index_name>
        ```


## Precautions

-   You can migrate the nodes only in a single zone to another zone at a time. If you want to migrate the nodes in multiple zones at a time, you must perform migration multiple times.
-   After you migrate the nodes in a zone to another zone, the system performs a rolling restart for your cluster. The time required for the migration depends on the size, data volume, and load of your cluster. We recommend that you perform migration during off-peak hours.
-   In most cases, if the indexes of your cluster have replica shards and the load of your cluster is normal, your cluster can still provide services during migration. The following items indicate that the load of a cluster is normal: The CPU utilization of the cluster is about 60%, the heap memory usage of the cluster is about 50%, and the value of NodeLoad\_1m is less than the number of vCPUs for the cluster.
-   If the indexes of your cluster do not have replica shards, the load of the cluster is excessively high, and large amounts of data are written to or queried in your cluster, access timeouts may occur during migration. We recommend that you configure an access retry mechanism for your client before you migrate nodes in a zone. This reduces the impact on your business.

## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  On the **Basic Information** page, click the **Node Visualization** tab in the lower part. Then, move the pointer over the zone and click **Migrate**.

    ![Migrate nodes to another zone](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8367819951/p77306.png)

5.  In the **Node Migration** dialog box, specify **Target Zone** and **vSwitch**.

    ![Node Migration dialog box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0527141261/p77329.png)

    |Parameter|Description|
    |---------|-----------|
    |**Target Zone**|The destination zone may also have insufficient resources. We recommend that you select the destination zone from bottom to top in alphabetical order. For example, if both **cn-hangzhou-e** and **cn-hangzhou-h** are available, select **cn-hangzhou-h**.|
    |**vSwitch**|If your cluster is a single-zone cluster, you must select a new **vSwitch** for node migration. If your cluster is a multi-zone cluster or a cluster deployed on the Alibaba Finance Cloud, you do not need to select a new **vSwitch**.|

    **Note:**

    -   After the nodes in the zone are migrated, the IP addresses of the nodes change. If you specified the IP addresses in the configuration of your cluster, you must update the IP addresses after the migration.
    -   Node migration triggers a cluster restart. During the restart, the cluster can still provide services, but the services may be unstable. Therefore, we recommend that you migrate nodes during off-peak hours.
6.  Read and agree to the terms of data migration, and click **OK**.

    Then, the system restarts the cluster. After the cluster is restarted, the nodes are migrated to the destination zone.

    **Note:** After the migration is complete, the nodes are migrated to a new zone. After the migration, the original zone is still displayed on the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) and Upgrade/Downgrade pages in the Elasticsearch console. This does not affect the use of the cluster in the new zone. You can view the actual zone where the cluster resides on the [Node Visualization](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the cluster status and node information.md) tab of the Basic Information page.


