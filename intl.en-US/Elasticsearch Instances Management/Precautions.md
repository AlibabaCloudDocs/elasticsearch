---
keyword: [cross-zone Elasticsearch cluster, nodes in a cross-zone Elasticsearch cluster, index replicas of a cross-zone Elasticsearch cluster, configuration of a cross-zone Elasticsearch cluster, switchover of a cross-zone Elasticsearch cluster, recovery of a cross-zone Elasticsearch cluster]
---

# Precautions

A cross-zone Elasticsearch cluster offers improved disaster recovery capabilities. This topic describes the precautions for deploying and using a cross-zone Elasticsearch cluster.

When you purchase an Elasticsearch cluster, you can select the number of zones for it. If you select two or three zones, the system deploys the cluster across these zones. During deployment, you do not need to select each zone. The system automatically selects the zones. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

**Note:** Currently, you can deploy an Elasticsearch cluster across three zones only in the China \(Hangzhou\), China \(Beijing\), China \(Shanghai\), or China \(Shenzhen\) region.

## Nodes

-   You must purchase dedicated master nodes.
-   The numbers of data nodes, warm nodes, and client nodes must be a multiple of the number of zones. For more information about zones, see [Regions and zones](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md).

## Index replicas

-   If your Elasticsearch cluster is deployed across two zones but one zone fails, the remaining zone needs to continue providing services. Therefore, you must configure at least one replica for each index.

    By default, five primary shards and one replica are enabled for each index. If you do not have specific requirements on read performance, you can use the default setting.

-   If your Elasticsearch cluster is deployed across three zones but one or two of them fail, the remaining zones need to continue providing services. Therefore, you must configure at least two replicas for each index.

    By default, five primary shards and one replica are enabled for each index. Therefore, you must modify the index template to adjust the default number of replicas. For more information, see [Index Templates](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/indices-templates.html). The following example code demonstrates how to modify the index template to set the number of replicas to 2:

    ```
    PUT _template/template_1
    {
      "template": "*",
      "settings": {
        "number_of_replicas": 2
      }
    }                                
    ```


## Cluster configuration

The system automatically configures shard allocation awareness policies for cross-zone Elasticsearch clusters. For more information, see [Shard allocation awareness](https://www.elastic.co/guide/en/elasticsearch/reference/master/allocation-awareness.html).

The following table lists the parameter configurations of an Elasticsearch cluster deployed across the cn-hangzhou-f and cn-hangzhou-g zones.

|Parameter|Description|Example value|
|---------|-----------|-------------|
|`cluster.routing.allocation.awareness.attributes`|**Note:** Do not call an Elasticsearch API operation to change the value of this parameter. Otherwise, exceptions may occur.

Specifies the node attributes that are used to configure a shard allocation awareness policy for a cross-zone Elasticsearch cluster. The cluster identifies the zone of a node based on the `Enode.attr.zone_id` parameter added to the startup parameter of the node. This parameter is fixed to `zone_id`.

**Note:** When you use a cross-zone Elasticsearch cluster, the system adds `-Enode.attr.zone_id` to the startup parameter of each node in the cluster. For example, if a node is deployed in the cn-hangzhou-g zone, the system adds `-Enode.attr.zone_id=cn-hangzhou-g` to the startup parameter of the node.

|`"zone_id"`|
|`cluster.routing.allocation.awareness.force.zone_id.values`|Specifies whether forced awareness is enabled for shard allocation. Assume that the index of an Elasticsearch cluster deployed across the cn-hangzhou-f and cn-hangzhou-g zones contains one primary shard and three replica shards. Based on the shard allocation awareness policy, the system allocates two shards to each of the cn-hangzhou-f and cn-hangzhou-g zones. If the `cluster.routing.allocation.awareness.force.zone_id.values` parameter is specified, when the cn-hangzhou-f zone fails, forced awareness prevents the system from reallocating the shards of the cn-hangzhou-f zone to the cn-hangzhou-g zone. **Note:** By default, this parameter is not specified. You can specify it as required.

|`["cn-hangzhou-f", "cn-hangzhou-g"]`|

## Switchover and recovery

After a cross-zone Elasticsearch cluster is deployed, you can perform the following operations for it:

-   If the nodes of your Elasticsearch cluster in a zone become faulty, you can perform a switchover to remove these nodes. For more information, see [Perform a switchover](/intl.en-US/Elasticsearch Instances Management/Cross-zone instance deployment/Perform a switchover.md).
-   After the faulty nodes recover, you can perform a recovery to add the nodes to the zone again. For more information, see [Perform a recovery](/intl.en-US/Elasticsearch Instances Management/Cross-zone instance deployment/Perform a recovery.md).

