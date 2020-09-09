---
keyword: [restart an Elasticsearch cluster, restart an Elasticsearch node]
---

# Restart a cluster or node

After you modify the configuration of or perform other operations on an Elasticsearch cluster or node, you may need to manually restart the cluster or node for the changes to take effect. This topic describes how to restart an Elasticsearch cluster or node in the Elasticsearch console.

Before you restart a cluster, make sure that the **state** of the cluster is **Active** \(indicated by the color green\), each index has at least one replica shard, and resource usage is not high.

**Note:** You can view the resource usage on the **Cluster Monitoring** page. For example, the value of **NodeCPUUtilization\(%\)** is about 80%, that of **NodeHeapMemoryUtilization** is about 50%, and that of **NodeLoad\_1m** is less than the number of vCPUs of the current data node. For more information, see [Monitoring metrics](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Cluster monitoring.md).

## Restart a cluster

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the upper-right corner of the **Basic Information** page, click **Restart**.

5.  In the **Restart** dialog box, specify the required parameters.

    ![Restart an Elasticsearch cluster](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3287659951/p81252.png)

    **Note:** In most cases, if the load of a cluster is not high and indexes have replica shards, the cluster can still provide services during a restart. In some cases, however, access timeouts may occur during a restart. For example, a number of clusters are forced to restart at the same time, the cluster is heavily loaded and is not accessible, indexes do not have replica shards, or large amounts of data is written and queried. In these cases, we recommend that you design a retry mechanism on the client before you restart the cluster.

    |Parameter|Description|
    |---------|-----------|
    |**Object**|**Cluster** and **Node** are supported.     -   **Cluster**: All nodes in a cluster are restarted.
    -   **Node**: A single node in a cluster is restarted. For more information, see [Restart a node](#section_ed1_ce1_s6k). |
    |**Restart Mode**|Elasticsearch provides two restart modes: **Restart** and **Force Restart**.     -   **Restart**: You can only restart a cluster whose **state** is **Active**. Otherwise, you must forcibly restart the cluster. This restart mode does not affect the service of your cluster but is time-consuming.

**Note:**

        -   During the restart, the CPU utilization and memory usage of the nodes in your Elasticsearch cluster increase sharply. This may affect the stability of your services for a short period of time.
        -   The specific amount of time that is required to restart a cluster depends on the volume of data stored on the cluster and the numbers of nodes, indexes, and shards in the cluster. You can view the restart progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box.
    -   **Force Restart**: If your Elasticsearch cluster is abnormal \(indicated by the color yellow or red\), you can use only this mode to restart the cluster.

**Note:** If the disk usage exceeds the value of `cluster.routing.allocation.disk.watermark.low`, the **state** of your Elasticsearch cluster may be displayed yellow or red. If your Elasticsearch cluster is abnormal, do not perform the following operations on the cluster: node addition, node capacity expansion, disk resizing, restart, password reset, and other operations that may change the configuration of the cluster. Perform the operations only after the **state** of the cluster is **Active**. |
    |**Concurrency**|You can set the concurrency of the cluster to improve the restart speed. The higher the concurrency, the faster a forced restart. The default concurrency is calculated by dividing one by the total number of nodes in the cluster.|
    |**Estimated to Take**|This value is calculated by multiplying the average time of the previous restarts by the total number of nodes. The actual restart time takes precedence.|

6.  Click **OK**.

    **Note:** If you choose Force Restart, select **Restart Cluster Forcibly** to confirm the restart.

    During the restart, the value of **Status** is **Initializing** \(indicated by the color yellow\). You can view details in the **Tasks** dialog box. After the cluster is restarted, the value of **Status** is **Active**.

    ![Restart progress](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3287659951/p59938.png)


## Restart a node

Restart a single node. The procedure and precautions for restarting a node are similar to those for [restarting a cluster](#section_rc0_xb0_vnt). The following content describes the differences.

![Restart a node](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3287659951/p75998.png)

-   In the **Restart** dialog box, set **Object** to **Node**.
-   Select the node you want to restart.

    **Note:** If your Elasticsearch cluster is abnormal, you must forcibly restart the nodes in the cluster.

-   The system provides the **Change Blue-green Release** feature. If you select **Change Blue-green Release**, Elasticsearch adds a node to your Elasticsearch cluster, migrates the data on the original node to the new node, and removes the original node. If a node experiences a hardware failure, you can use the **Change Blue-green Release** feature to remove the node.

    **Warning:**

    -   If you use the **Change Blue-green Release** feature, make sure that your Elasticsearch cluster is in the **Active** state and do not set **Restart Mode** to **Force Restart**.
    -   If you select **Change Blue-green Release**, the IP address of the node changes after the restart.

