---
keyword: [Elasticsearch physical replication, Elasticsearch apack]
---

# Use the physical replication feature of the apack plug-in

The apack plug-in is developed by the Alibaba Cloud Elasticsearch team. This plug-in provides the physical replication and vector retrieval features. This topic describes only the physical replication feature. This feature greatly reduces CPU overheads and improves write performance in scenarios such as logging and time series analytics. In these scenarios, replica shards are configured for indexes, large amounts of data are written, and data visibility is latency-insensitive.

-   An Alibaba Cloud Elasticsearch V6.7.0 or V 7.10.0 cluster is created. The kernel version of the V6.7.0 cluster is 1.2.0 or later. In this topic, an Elasticsearch V6.7.0 cluster is used. For more information about how to create an Elasticsearch cluster, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).
-   The apack plug-in is installed.

    Only Elasticsearch V 6.7.0 and 7.10.0 clusters support the apack plug-in. If you use an Elasticsearch V6.7.0 cluster and the kernel version of the cluster is earlier than 1.2.0, you must update the kernel before you use the apack plug-in. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md). If the kernel version of your V6.7.0 cluster is 1.2.0 or later, this plug-in is already installed on your cluster by default and cannot be removed. You can go to the [Plug-ins](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Overview.md) page to check whether the plug-in is installed.

    **Note:** After the apack plug-in is installed, you can use both the physical replication and vector retrieval features. For more information about how to use the vector retrieval feature, see [Use the aliyun-knn plug-in]().


Basic principle of the physical replication feature: If the feature is disabled, the system writes index data to a primary shard when the node where the primary shard resides receives a write request. Then, the system synchronizes the request to the nodes where the replica shards of the primary shard reside and writes the index data to the replica shards. This process is the same as that in open source Elasticsearch. In this process, index data is written to not only the primary shard and its replica shards but also their translogs. After the feature is enabled, index data is written to the primary shard, its translogs, and the translogs of its replica shards. This ensures data reliability and consistency. Each time the primary shard is refreshed, the system copies incremental index data to the replica shards of the primary shard over the network. This feature delays data visibility but significantly improves the write performance of a cluster.

Performance testing of the physical replication feature:

-   Test environment
    -   Node configuration: 5 data nodes \(each with 8 vCPUs and 32 GiB of memory\) and one 2-TiB standard SSD
    -   Dataset: 74-GiB nyc\_taixs of Rally provided by open source Elasticsearch
    -   Index configuration: five primary shards and one replica shard for each primary shard \(default configuration\)
-   Test result

    |Service|Write speed \(document/s\)|
    |-------|--------------------------|
    |Open source Elasticsearch 6.7.0|127305|
    |Alibaba Cloud Elasticsearch V6.7.0 \(with the physical replication feature enabled\)|184592|

-   Test conclusion

    Alibaba Cloud Elasticsearch with the physical replication feature enabled delivers a write performance 45% better than open source Elasticsearch.


**Note:** You can run the commands provided in this topic in the Kibana console. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Precautions

-   The physical replication feature of the apack plug-in works on indexes. By default, this feature is disabled for indexes created before the plug-in is installed and is enabled for indexes created after the plug-in is installed. If your indexes are created before the plug-in is installed, you must enable the feature before you can use it.
-   You can disable the physical replication feature for an index. However, before you disable this feature, disable the index.
-   Before you enable the physical replication feature for an index, disable the index and set the number of replica shards for the index to 0.

## Enable the physical replication feature for a new index

When you create an index, use the settings configuration to enable the physical replication feature for the index.

```
PUT index-1
{
"settings": {
   "index.replication.type" : "segment"
 }
}
```

## Disable the physical replication feature for an index

1.  Disable the index.

    ```
    POST index-1/_close
    ```

2.  Disable the physical replication feature.

    ```
    PUT index-1/_settings
    {
      "index.replication.type" : null
    }
    ```

3.  Enable the index.

    ```
    POST index-1/_open
    ```


## Enable the physical replication feature for an existing index

1.  Set the number of replica shards for the index to 0.

    ```
    PUT index-1/_settings
    {
      "index.number_of_replicas": 0
    }
    ```

2.  Disable the index.

    ```
    POST index-1/_close
    ```

3.  Enable the physical replication feature.

    ```
    PUT index-1/_settings
    {
      "index.replication.type" : "segment"
    }
    ```

4.  Enable the index.

    ```
    POST  index-1/_open
    ```

5.  Set the number of replica shards to 1.

    ```
    PUT index-1/_settings
    {
      "index.number_of_replicas": 1
    }
    ```


