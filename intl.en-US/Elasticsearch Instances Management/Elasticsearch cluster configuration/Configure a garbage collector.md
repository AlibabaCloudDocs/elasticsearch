---
keyword: [configure a garbage collector for an Elasticsearch cluster, CMS garbage collector, G1 garbage collector]
---

# Configure a garbage collector

If the memory size of each data node in your Alibaba Cloud Elasticsearch cluster of V6.7.0 or later is 32 GiB or larger, you can configure a garbage collector for the cluster. You can also change the garbage collector type for the cluster. The following types of garbage collectors are supported: **CMS** and **G1**. This topic describes how to configure a garbage collector.

An Alibaba Cloud Elasticsearch cluster is created. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md). The cluster must meet the following requirements:

-   Version: V6.7.0 or later
-   Memory size of each data node: 32 GiB or larger

If the specifications of your cluster do not meet the preceding requirements, upgrade the cluster configuration. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

**Note:** Elasticsearch clusters that do not meet the preceding requirements can use only **CMS garbage collectors**. In addition, you cannot switch the garbage collectors to **G1 garbage collectors** for the clusters.

## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  In the **Basic Configuration** section, click **Modify** next to **Garbage Collectors**.

    You can modify the configuration of a garbage collector for a cluster only after the cluster meets the following requirements:

    -   Version: V6.7.0 or later
    -   Memory size of each data node: 32 GiB or larger
    **Warning:**

    -   Before you modify the configuration of a garbage collector for a cluster, you must make sure that the cluster is in a normal state. After you modify the configuration of a garbage collector for a cluster, the system restarts the cluster. The time required for the restart depends on the size, data volume, and load of the cluster. We recommend that you make the modification during off-peak hours.
    -   In most cases, if the indexes of your cluster have replica shards and the load of your cluster is normal, your cluster can still provide services during a cluster modification. The following items indicate that the load of a cluster is normal: The CPU utilization of the cluster is about 60%, the heap memory usage of the cluster is about 50%, and the value of NodeLoad\_1m is less than the number of vCPUs for the cluster.

    -   If the indexes of your cluster do not have replica shards, the load of the cluster is excessively high, and large amounts of data are written to or queried in your cluster, access timeouts may occur during a cluster modification. We recommend that you configure an access retry mechanism for your client before you perform a cluster modification. This reduces the impact on your business.

6.  In the **Edit Configuration** panel, select **G1** and click **OK**.

    ![Change the garbage collector type](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7467819951/p57961.png)

    After you confirm the operation, the system restarts the cluster. After the cluster is restarted, the garbage collector is switched to G1.


## References

[UpdateAdvancedSetting](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateAdvancedSetting.md)

