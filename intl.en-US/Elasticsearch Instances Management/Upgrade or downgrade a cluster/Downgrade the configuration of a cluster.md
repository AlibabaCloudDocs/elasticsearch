# Downgrade the configuration of a cluster

If your business has traffic peak and off-peak hours, the node specifications of your Elasticsearch cluster may be higher than those required by your business during off-peak hours. In this case, you can downgrade the configuration of your cluster to reduce costs. You can downgrade nodes or change the disk types of nodes. This topic describes how to downgrade the configuration of an Elasticsearch cluster and provides the related precautions and instructions.

-   The Elasticsearch cluster whose configuration you want to downgrade is in the Active state \(indicated by the color green\).
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


## Limits

You are not allowed to reduce disk space or downgrade Kibana nodes.

## Precautions

-   After you downgrade the configuration of your Elasticsearch cluster, the system restarts the cluster to make the changes take effect. The time required for the restart depends on the specifications, data structure, and data volume of the cluster. In most cases, the restart requires a few hours. Therefore, to reduce the impact of the downgrade on your business, we recommend that you perform the downgrade during off-peak hours. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).

    **Note:**

    -   In most cases, if the indexes of your cluster have replica shards and the load of your cluster is normal, your cluster can still provide services during a cluster configuration change. The following items indicate that the load of a cluster is normal: The CPU utilization of the cluster is about 60%, the heap memory usage of the cluster is about 50%, and the value of NodeLoad\_1m is less than the number of vCPUs for the cluster.
    -   If the indexes of your cluster do not have replica shards, the load of the cluster is excessively high, and large amounts of data are written to or queried in your cluster, access timeouts may occur during a cluster configuration change. We recommend that you configure an access retry mechanism for your client before you perform a cluster configuration change. This reduces the impact on your business.
-   Before you downgrade the configuration of your Elasticsearch cluster, perform the operations provided in [Evaluate specifications and storage capacity]() and make sure that the conditions provided in [Conditions for configuration downgrades](#section_hmw_quu_exd) are met. This ensures that your cluster has sufficient storage after the downgrade. If the cluster load is high after the downgrade, we recommend that you upgrade the configuration at the earliest opportunity. For more information about how to upgrade the configuration of a cluster, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).
-   Configuration downgrades may cause additional risks, such as data loss, for clusters with non-standard specifications, such as a cluster that contains only two data nodes. Therefore, proceed with caution.

## Conditions for configuration downgrades

The configuration of a cluster can be downgraded only if the following conditions are met:

-   Downgrade interval

    The interval between two configuration downgrades of a cluster must be no less than 30 minutes.

-   Cluster load

    A cluster may contain multiple types of nodes. The requirements for the current CPU utilization and JVM heap memory usage vary based on node types. The following table lists the requirements.

    |Node type|Current CPU utilization|Current JVM heap memory usage|
    |---------|-----------------------|-----------------------------|
    |Dedicated master node|Maximum value of a single node: < 30%|Maximum value of a single node: < 25%|
    |Data node|Both of the following conditions must be met:    -   Maximum value of a single node: < 60%
    -   Average value of all nodes: < 40%
|Both of the following conditions must be met:    -   Maximum value of a single node: < 50%
    -   Average value of all nodes: < 30% |
    |Client node|Both of the following conditions must be met:    -   Maximum value of a single node: < 50%
    -   Average value of all nodes: < 30%
|Both of the following conditions must be met:    -   Maximum value of a single node: < 50%
    -   Average value of all nodes: < 30% |
    |Warm node|Both of the following conditions must be met:    -   Maximum value of a single node: < 60%
    -   Average value of all nodes: < 40%
|Both of the following conditions must be met:    -   Maximum value of a single node: < 50%
    -   Average value of all nodes: < 30% |

-   Specifications

    The selected specifications for vCPUs and memory must be greater than or equal to half of the current specifications.


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region.

4.  On the Elasticsearch Clusters page, find your cluster and choose **More** \> **Downgrade Configuration** in the **Actions** column.

5.  Change the specifications or disk type of the nodes that you want to downgrade.

6.  Read and select Elasticsearch Agreement of Service, and click **Buy Now**.

    After you complete the payment, the system restarts the cluster to make the changes take effect.


