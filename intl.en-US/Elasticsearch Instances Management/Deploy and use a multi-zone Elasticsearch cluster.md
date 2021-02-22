# Deploy and use a multi-zone Elasticsearch cluster

A multi-zone Elasticsearch cluster offers improved disaster recovery capabilities. The system automatically selects the zones that have sufficient ECS instances to deploy a cluster. If replica shards are configured for indexes in the cluster and nodes in one zone fail, the nodes in the remaining zones can still provide services without interruption. This significantly enhances the availability of the cluster. In addition, you can perform a switchover in the Elasticsearch console to isolate the faulty nodes. Then, the system adds computing resources to the remaining zones to make up for the resources lost in the zone that contains the faulty nodes. This topic describes how to deploy a multi-zone Elasticsearch cluster, perform a switchover for faulty nodes in a zone, and recover the faulty nodes in the zone.

## Scenarios

You can deploy an Alibaba Cloud Elasticsearch cluster by using one of the following methods:

-   In one zone: This is the default deployment method. In most cases, it is used to handle non-critical workloads.
-   Across two zones: This deployment method implements cross-zone disaster recovery. In most cases, it is used to handle production workloads.
-   Across three zones: This deployment method implements high availability. We recommend that you use this deployment method to handle production workloads that have high requirements for service availability.

## Deploy a multi-zone Elasticsearch cluster

-   Operations

    When you purchase an Alibaba Cloud Elasticsearch cluster, you can select the number of zones for the cluster. If you select two or three zones, the system deploys the cluster across these zones. During the deployment, you do not need to select each zone. The system automatically selects the zones. For more information, see [Create a cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create a cluster.md) and [Parameters on the buy page](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md).

    **Note:** If you choose to deploy a cluster across zones, the console displays only the zones where nodes that receive network traffic from clients are deployed, such as Hangzhou Zone J. The system actually deploys the cluster to the zones that have sufficient ECS instances, such as Beijing Zone H and Beijing Zone J.

-   Precautions

    |Category|Precaution|
    |--------|----------|
    |Nodes|    -   You must purchase three dedicated master nodes.
    -   The number of data nodes, warm nodes, and client nodes must be a multiple of the number of zones. For more information about zones, see [Regions and zones](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md).
    -   If you choose to deploy your cluster across two zones, Alibaba Cloud Elasticsearch uses one of the following methods to deploy your cluster:
        -   If the current region has at least three zones, and all these zones have sufficient ECS instances, the dedicated master nodes of the cluster are deployed in these zones. In this case, if nodes in one zone fail, your Elasticsearch cluster can still select a dedicated master node.
        -   If the current region has only two zones or only two zones in the region have sufficient ECS instances, the dedicated master nodes are deployed in the two zones. If nodes in the zone that contains only one dedicated master node fail, your Elasticsearch cluster can still select a dedicated master node. If nodes in the zone that contains two dedicated master nodes fail, you must perform a switchover in the Elasticsearch console. |
    |Replica shards of indexes|    -   If an Elasticsearch cluster is deployed across two zones and one zone becomes unavailable, the other zone is used to provide services. Therefore, you must configure no less than one replica shard for each primary shard of an index.

By default, one replica shard is configured for each primary shard of an index. If you do not have specific requirements on read performance, use the default settings.

    -   If an Elasticsearch cluster is deployed across three zones and one or two of the zones become unavailable, the remaining zones are used to provide services. Therefore, you must configure no less than two replica shards for each primary shard of an index.

By default, one replica shard is configured for each primary shard of an index. Therefore, you must modify the number of replica shards in the index template. For more information, see [Index Templates](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/indices-templates.html). In the following code, the number of replica shards is set to 2:

        ```
PUT _template/template_1
{
  "template": "*",
  "settings": {
    "number_of_replicas": 2
  }
}                                
        ``` |

-   Configuration description

    During the deployment of the cluster, the system automatically enables shard allocation awareness for the cluster. For more information, see [Shard allocation awareness](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-cluster.html#shard-allocation-awareness). The following table describes the parameters that are configured for an Elasticsearch cluster deployed across the cn-hangzhou-f and cn-hangzhou-g zones.

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |cluster.routing.allocation.awareness.attributes|**Note:** Do not call an Elasticsearch API operation to change the value of this parameter. Otherwise, exceptions may occur.

Specifies the node attributes that are used to enable shard allocation awareness for the cluster. The system adds the Enode.attr.zone\_id parameter to the launch settings of a node in a multi-zone cluster to identify the zone of the node. For example, a node of a multi-zone cluster is deployed in the cn-hangzhou-g zone. In this case, the system adds `-Enode.attr.zone_id=cn-hangzhou-g` to the launch settings of the node, and the value of the Enode.attr.zone\_id parameter is fixed to zone\_id.

|zone\_id|
    |cluster.routing.allocation.awareness.force.zone\_id.values|Enables or disables forced awareness for shard allocation. Forced awareness prevents a zone from being overloaded when other zones fail. For example, the index of an Elasticsearch cluster deployed across the cn-hangzhou-f and cn-hangzhou-g zones has one primary shard and three replica shards. Based on the shard allocation awareness policy, the system allocates two shards to each of the cn-hangzhou-f and cn-hangzhou-g zones. If the cluster.routing.allocation.awareness.force.zone\_id.values parameter is specified and the cn-hangzhou-f zone becomes unavailable, forced awareness prevents the system from reallocating the shards of the cn-hangzhou-f zone to the cn-hangzhou-g zone. **Note:** By default, this parameter is not specified. You can specify the parameter based on your business requirements.

|\["cn-hangzhou-f", "cn-hangzhou-g"\]|


## Perform a switchover and recover faulty nodes

If your Elasticsearch cluster is deployed across zones and the nodes in one zone become faulty, you can perform a switchover. The system removes the nodes from this zone and transmits the network data sent from clients to only nodes in the other zones that are in the Enabled state. After the faulty nodes in the zone where a switchover is performed recover, you can perform a recovery. The system adds the nodes removed during the switchover to the zone again, and transmits the network data sent from clients to nodes in all the zones that are in the Enabled state.

**Note:** Before the switchover, you must make sure that the indexes in the cluster have replica shards. This ensures normal read and write operations on the cluster after the switchover.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  On the **Node Visualization** tab of the **Basic Information** page of the cluster, perform a switchover.

    1.  Move the pointer over the zone for which a switchover needs to be performed and click **Switch Over**.

        ![Switchover](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8900628061/p88646.png)

    2.  In the **Confirm Operation** message, click **OK**.

        Then, the system restarts your Elasticsearch cluster to make the switchover take effect. After the switchover succeeds, the state of the zone changes from Enabled to Disabled.

        **Note:** When you perform a switchover, the system automatically adds dedicated master nodes, client nodes, and data nodes to the remaining zones in the Enabled state. This ensures that your Elasticsearch cluster has sufficient computing resources and the read and write operations on indexes are not affected.

    If your index has replica shards before you perform the switchover, the state of your cluster becomes abnormal \(indicated by the color yellow\) after the switchover. In this case, after the switchover is complete, you can [log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) and configure parameters for the cluster by referring to the following command. This operation is used to allocate the shards in the zone for which the switchover is performed to the remaining zones. After the shards are allocated, the state of the cluster becomes normal \(indicated by the color green\).

    ```
    PUT /_cluster/settings
    {
        "persistent" : {
            "cluster.routing.allocation.awareness.force.zone_id.values" : {"0": null, "1": null, "2": null}
        }
    }
    ```

5.  On the **Node Visualization** tab, recover the nodes in the zone for which the switchover is performed.

    1.  Move the pointer over the zone whose nodes you want to recover and click **Switch Back**.

    2.  In the **Confirm Operation** message, click **OK**.

        Then, the system restarts your Elasticsearch cluster to make the recovery take effect. After the recovery succeeds, the state of the zone changes from Disabled to Enabled.

        **Note:** When you recover the nodes in the zone, the system removes the dedicated master nodes, client nodes, and data nodes that were added during the switchover. In addition, the system migrates the data stored on the removed data nodes to other data nodes.


