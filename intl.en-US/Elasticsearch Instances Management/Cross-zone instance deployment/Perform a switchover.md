---
keyword: switchover
---

# Perform a switchover

If your Elasticsearch cluster is deployed across zones and the nodes in a zone become faulty, you can perform a switchover. The system removes the nodes from this zone and transmits the network data sent from clients only to nodes in the other zones that are in the Enabled state. This topic describes how to perform a switchover.

You have completed the following operations:

-   A multi-zone cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md). When you create a cluster, set **Number of Zones** to **2-AZ** or **3-AZ**.

    **Note:** You can deploy an Elasticsearch cluster across three zones only in the China \(Hangzhou\), China \(Beijing\), China \(Shanghai\), or China \(Shenzhen\) region.

-   Replicas are configured for the indexes of the multi-zone Elasticsearch cluster. This ensures normal read and write operations on the cluster after a switchover.

## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the lower part of the **Basic Information** page, click the **Node Visualization** tab. Then, move the pointer over the zone where you want to perform a switchover and click **Switch Over**.

    ![Switchover](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8900628061/p88646.png)

5.  In the **Confirm Operation** dialog box, click **OK**.

    The system then restarts your Elasticsearch cluster to make the switchover take effect. After the switchover succeeds, the state of the zone changes from Enabled to Disabled.

    **Note:** To ensure that your Elasticsearch cluster has sufficient computing resources and the read and write operations on indexes are not affected, the system adds nodes to the zones that are in the Enabled state during a switchover. These nodes may include dedicated master nodes, client nodes, and data nodes.


If the indexes of the cluster have replicas before you perform a switchover, the state of your cluster becomes abnormal \(indicated by the color yellow\) after the switchover. After you confirm that the switchover succeeds, you can run a command in the Kibana console to configure cluster parameters. This way, the shards in the zone where the switchover is performed can be allocated to the zones that are in the Enabled state. After the shards are allocated, the state of the cluster becomes normal \(indicated by the color green\). Example command:

```
PUT /_cluster/settings
{
    "persistent" : {
        "cluster.routing.allocation.awareness.force.zone_id.values" : {"0": null, "1": null, "2": null}
    }
}
```

**Note:** For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

