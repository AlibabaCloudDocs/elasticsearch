# View the cluster status and node information

You can view the status of your Alibaba Cloud Elasticsearch cluster, basic information of its nodes, and node configurations on the Node Visualization or Configuration Info tab of the cluster details page. The basic information includes the IP address, status, CPU utilization, memory size, disk usage, and Java Virtual Machine \(JVM\) heap memory usage. This topic describes how to view the cluster status and node information.

## Go to the Node Visualization tab

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  On the **Basic Information** page, click the **Node Visualization** tab.


## View the cluster status and health diagnostic report

1.  [Go to the Node Visualization tab](#section_952_y3f_die).

2.  Move the pointer over **Cluster**. In the popover that appears, view the cluster status.

    ![View the cluster status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7367819951/p88678.png)

3.  Click **Intelligent Maintenance** in the popover to view the health diagnostic report of the cluster.

    For more information, see [Overview](/intl.en-US/Elasticsearch Instances Management/Operation and Maintenance/Intelligent operations and maintenance/Overview.md).


## View the basic information of nodes

1.  [Go to the Node Visualization tab](#section_952_y3f_die).

2.  View the colors of nodes and determine the health statuses of the nodes based on the colors.

    ![Health statuses of nodes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7367819951/p57677.png)

    **Note:** The color of a node is determined by the resource usage of the node. The resource usage thresholds are the same as those in Cloud Monitor. For more information, see [Monitoring metrics](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Monitoring metrics.md).

    -   Red: Warning.
    -   Yellow: Alert.
    -   Green: Normal.
    -   Gray: Unknown. This state indicates that the system failed to retrieve the information of a node for a long period.
3.  Move the pointer over a node. In the popover that appears, view the information of the node.

    ![View node information](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8367819951/p57679.png)

    **Note:** If a node is in red or yellow color, the system displays the **The nodes are not running in the Normal state. We recommend that you use Intelligent Maintenance to check your cluster** message. If a node is in gray color, the system displays the **The node is disconnected, and we recommend that you use Intelligent Maintenance to check your cluster** message. You can click **Intelligent Maintenance** to go to the **Intelligent Maintenance** \> **Cluster Diagnosis** page. Then, diagnose the cluster. For more information, see [Overview](/intl.en-US/Elasticsearch Instances Management/Operation and Maintenance/Intelligent operations and maintenance/Overview.md).

4.  Click **Restart**. In the **Restart** dialog box, configure the parameters to restart the node.

    For more information about the detailed operations and precautions, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).


## View the configurations of nodes

1.  [Go to the Node Visualization tab](#section_952_y3f_die).

2.  Click the **Configuration Info** tab.

3.  On the **Configuration Info** tab, view the configurations of the nodes.

    ![Configuration Info tab](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9367819951/p50251.png)

    For more information, see [Parameters on the buy page](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md).


## References

[ListAllNode](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ListAllNode.md)

