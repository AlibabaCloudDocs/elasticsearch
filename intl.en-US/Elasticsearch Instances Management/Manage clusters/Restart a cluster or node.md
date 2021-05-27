---
keyword: [restart an Elasticsearch cluster, restart an Elasticsearch node]
---

# Restart a cluster or node

After you modify the configuration of or perform other operations on an Elasticsearch cluster or node, you may need to restart it to make the changes take effect. This topic describes how to restart an Elasticsearch cluster or node in the Elasticsearch console.

-   The state of the cluster is **Active** \(indicated by the color green\), each index has at least one replica shard, and resource usage is not high.

    **Note:** You can view the resource usage on the **Cluster Monitoring** page. For example, the value of **NodeCPUUtilization\(%\)** is about 80%, that of **NodeHeapMemoryUtilization** is about 50%, and that of **NodeLoad\_1m** is less than the number of vCPUs for the current data node. For more information, see [Monitoring metrics](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Monitoring metrics.md).

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


## Restart a cluster

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  In the upper-right corner of the **Basic Information** page, click **Restart**.

5.  In the **Restart** dialog box, configure the parameters.

    ![Restart an Elasticsearch cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3287659951/p81252.png)

    **Note:** In most cases, if the load of a cluster is not high and indexes have replica shards, the cluster can still provide services during a restart. In some cases, however, the access to a cluster that is being restarted may time out. For example, if a number of clusters are forced to restart at the same time, your cluster is heavily loaded and is not accessible, indexes on your cluster do not have replica shards, or large amounts of data are written or queried in your cluster, the access to your cluster may time out. In these cases, we recommend that you design a retry mechanism on your client before you restart a cluster.

    |Parameter|Description|
    |---------|-----------|
    |**Object**|**Cluster** and **Node** are supported.     -   **Cluster**: All the nodes in the cluster are restarted.
    -   **Node**: A single node in the cluster is restarted. For more information, see [Restart a node](#section_ed1_ce1_s6k). |
    |**Restart Mode**|Elasticsearch provides two restart modes: **Restart** and **Force Restart**.     -   **Restart**: You can use this mode only when the cluster is in the **Active** state. If your cluster is not in this state, you must use the Force Restart mode. If you select the Restart mode, your cluster can still provide services during the restart, but the restart is time-consuming.

**Note:**

        -   During the restart, the CPU utilization and memory usage of the nodes in your cluster sharply increase. This may affect service stability for a short period of time.
        -   The time that is required to restart a cluster depends on the volume of data stored on the cluster and the numbers of nodes, indexes, and shards in the cluster. You can view the restart progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box.
    -   **Force Restart**: If your cluster is in an abnormal state \(indicated by the color yellow or red\), you can use only this mode to restart the cluster.

**Note:** If the disk usage exceeds the value of `cluster.routing.allocation.disk.watermark.low`, your cluster may become abnormal. If your cluster is in an abnormal state, do not perform the following operations on the cluster: node addition, node capacity expansion, disk resizing, restart, password reset, and other operations that may change the configuration of the cluster. Perform the preceding operations only after the state of the cluster becomes **Active**. |
    |**Concurrency**|You can set the concurrency of the cluster to improve the restart speed. The higher the concurrency, the faster a forced restart. The default concurrency is calculated by dividing one by the total number of nodes in the cluster.|

    **Note:** The value of **Estimated to Take** is calculated by multiplying the average time of the previous restarts by the total number of nodes. The actual restart time takes precedence.

6.  Click **OK**.

    **Note:** If you select the Force Restart mode, select **Restart Cluster Forcibly** to confirm the restart.

    During the restart, the value of **Status** is **Initializing** \(indicated by the color yellow\). You can view the details in the **Tasks** dialog box. After the cluster is restarted, the value of **Status** becomes **Active**.

    ![Restart progress](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3287659951/p59938.png)


## Restart a node

Restart a single node. The procedure and precautions for restarting a node are similar to those for [restarting a cluster](#section_rc0_xb0_vnt). The following descriptions provide the differences.

![Restart a node](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3287659951/p75998.png)

-   In the **Restart** dialog box, set **Object** to **Node**.
-   Select the node that you want to restart.

    **Note:** If the cluster to which the node you want to restart belongs is in an abnormal state, you must forcibly restart the node.

-   The system provides the **Change Blue-green Release** feature. If you select **Change Blue-green Release**, the system adds a node to your cluster, migrates the data on the original node to the new node, and removes the original node during the restart. If a node experiences a hardware failure, you can use the **Change Blue-green Release** feature to remove the node.

    **Warning:**

    -   If you want to use the **Change Blue-green Release** feature, make sure that your cluster is in the **Active** state. In addition, you must set **Restart Mode** to **Restart**.
    -   If you select **Change Blue-green Release**, the IP address of the node changes after the restart.

