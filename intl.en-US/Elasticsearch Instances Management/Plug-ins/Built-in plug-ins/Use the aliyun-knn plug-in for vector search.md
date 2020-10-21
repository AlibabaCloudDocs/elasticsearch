---
keyword: [aliyun-knn, vector search plug-in]
---

# Use the aliyun-knn plug-in for vector search

The aliyun-knn plug-in is a vector search engine designed by the Alibaba Cloud Elasticsearch team. It uses the vector databases of Proxima, a vector search engine designed by Alibaba DAMO Academy. This plug-in allows you to use vector spaces in different search scenarios, such as searching images, performing video fingerprinting, conducting both facial and speech recognition, and recommending commodities based on your preferences.

You have completed the following operations:

-   Install the aliyun-knn plug-in. Whether this plug-in is installed by default is determined by the Elasticsearch cluster version and kernel version.

    -   If the cluster version is V6.7.0 and the kernel version is V1.2.0 or later, the plug-in is integrated into the apack plug-in. The apack plug-in is installed by default. If you want to install or remove the aliyun-knn plug-in, you must perform operations on the apack plug-in. For more information, see [Use the physical replication feature of the apack plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md).
    -   If the cluster version is later than V6.7.0, or the cluster version is V6.7.0 and the kernel version is earlier than V1.2.0, you must manually install the aliyun-knn plug-in. For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Install and remove a built-in plug-in.md).
    **Note:**

    -   Only Elasticsearch clusters of V6.7.0 or later support the aliyun-knn plug-in.
    -   Before you install the aliyun-knn plug-in, make sure that each data node offers at least 2 vCPUs and 8 GiB of memory. These specifications are only for tests. For a production environment, the minimum specifications are 4 vCPUs and 16 GiB of memory. If your Elasticsearch cluster does not meet these requirements, upgrade the data nodes. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).
-   Perform [index planning](#section_x17_tbz_22y) and [cluster sizing](#section_ae0_2bt_ki4).

The vector search engine of Alibaba Cloud Elasticsearch is used in numerous production scenarios inside Alibaba Group, such as Pailitao, Image Search, Youku video fingerprinting, Qutoutiao video fingerprinting, Taobao commodity recommendation, customized searches, and Crossmedia searches.

Alibaba Cloud Elasticsearch provides the aliyun-knn plug-in for you to use its vector search engine. This plug-in is compatible with all open source Elasticsearch versions. Therefore, you do not need to learn how to use the engine. In addition to real-time incremental synchronization and near-real-time \(NRT\) searches, this engine supports other features of open source Elasticsearch in distributed searches. The features include multi-replica, restoration, and snapshots.

The vector search engine supports the Hierarchical Navigable Small World \(HNSW\) and Linear Search algorithms. These algorithms are suitable for processing small amounts of data from in-memory storage. The following table compares the performance of the two algorithms.

|Performance metric|HNSW|Linear Search|
|------------------|----|-------------|
|Top-10 recall ratio|98.6%|100%|
|Top-50 recall ratio|97.9%|100%|
|Top-100 recall ratio|97.4%|100%|
|Latency \(p99\)|0.093s|0.934s|
|Latency \(p90\)|0.018s|0.305s|

**Note:** p is short for percentage. For example, latency \(p99\) indicates how many seconds it requires to respond to 99% of queries.

## Index planning

|Algorithm|Use scenario|In-memory storage?|Remarks|
|---------|------------|------------------|-------|
|HNSW|-   Each node stores only small volumes of data.
-   A low response latency is required.
-   A high recall ratio is required.

|Yes|-   HNSW is based on the greedy search algorithm and obeys the triangle inequality. The triangle inequality states that the total sum of costs from A to B and from B to C must be greater than the costs from A to C. Inner product space does not obey the triangle inequality. Therefore, you must convert it to Euclidean space or spherical space before you apply the HNSW algorithm.
-   After you write data to Elasticsearch, we recommend that you regularly call the force merge API operation during off-peak hours to merge segments in shards. This can reduce the response latency. |
|Linear Search|-   Brute-force search.
-   A recall ratio of 100% is required.
-   The latency increases with the volume of data processed.
-   Effect comparison.

|Yes|None.|

## Cluster sizing

|Item|Description|
|----|-----------|
|Data node specifications \(required\)|The minimum data node specifications for a production environment are 4 vCPUs and 16 GiB of memory. The specifications of 2 vCPUs and 8 GiB of memory can be used only for tests.|
|Maximum volume of data per node|The maximum volume of data stored on each data node equals 50% of the total memory space of the data node.|
|Write throttling|Vector indexing is a CPU-intensive job. We recommend that you do not maintain a high write throughput. A peak write throughput lower than 5,000 TPS is recommended for a data node with 16 vCPUs and 64 GiB of memory. TPS is short for transactions per second. When Elasticsearch processes queries, it loads all index files to node memory. If nodes are out of memory, Elasticsearch reallocates shards. Therefore, we recommend that you do not write large amounts of data to Elasticsearch when it is processing queries. |

## Procedure

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab, run the following command to create an index:

    **Note:** The following sample code is applicable only to Alibaba Cloud Elasticsearch V6.7.0. For sample code that is applicable to Alibaba Cloud Elasticsearch V7.4.0, see [open source Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/getting-started-index.html).

    ```
    PUT test
    {
      "settings": {
        "index.codec": "proxima",
        "index.vector.algorithm": "hnsw" 
      },
      "mappings": {
        "_doc": {
          "properties": {
            "feature": {
              "type": "proxima_vector", 
              "dim": 2 
            },
            "id": {
              "type": "keyword"
            }
          }
        }
      }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |`index.vector.algorithm`|The algorithm. Valid values: `hnsw` and `linear`.|
    |`type`|The field type. Set the value to `proxima_vector` to specify a vector-type field.|
    |`dim`|The number of vector dimensions. Valid values: 1 to 2048.|

    The preceding sample code creates an index named `test`. The type of the index is `_doc`. The index contains two fields: `feature` and `id`. You can rename the index and fields as required.

4.  Run the following command to add a document:

    ```
    POST test/_doc
    {
      "feature": [1.0, 2.0], 
      "id": 1
    }
    ```

    **Note:** The value of the `feature` field must be a float array. The length of the array must be the same as that specified in the `dim` parameter in `mapping`.

5.  Run the following command to search for the document:

    ```
    GET test/_search
    {
      "query": {
        "hnsw": {  
          "feature": {
            "vector": [1.5, 2.5], 
            "size": 10 
          }
        }
      }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |`hnsw`|The value must be the same as that of the `algorithm` parameter specified when you create the index.|
    |`vector`|A float array. The length of the array must be the same as that specified in the `dim` parameter in `mapping`.|
    |`size`|The number of the top-ranked documents to return.|


## Parameters

|Parameter|Description|Default value|
|---------|-----------|-------------|
|`index.vector.algorithm`|The algorithm that you use to create an index. Valid values: hnsw and linear.|hnsw|

|Parameter|Description|Default value|
|---------|-----------|-------------|
|`index.vector.hnsw.builder.max_scan_num`|The maximum number of the nearest neighbors that you want to scan when a graph is created under the worst case.|100000|
|`index.vector.hnsw.builder.neighbor_cnt`|The maximum number of the nearest neighbors that each node can have at layer 0. We recommend that you set this parameter to 100. The quality of a graph increases with the value of this parameter. However, inactive indexes consume more storage resources.|100|
|`index.vector.hnsw.builder.upper_neighbor_cnt`|The maximum number of the nearest neighbors that each node can have on a layer other than layer 0. We recommend that you set this parameter to 50% of `neighbor_cnt`.|50|
|`index.vector.hnsw.builder.efconstruction`|The number of the nearest neighbors that you want to scan when a graph is created. The quality of a graph increases with the value of this parameter. However, a longer time period is required to create indexes. We recommend that you set this parameter to 400.|400|
|`index.vector.hnsw.builder.max_level`|The total number of layers, which includes layer 0. For example, you have 10 million documents and the `scaling_factor` parameter is set to 30. Use 30 as the base number and then round up the logarithm of 10,000,000 to the nearest integer. The result is 5.|6|
|`index.vector.hnsw.builder.scaling_factor`|A scaling factor. The volume of data on a layer equals the volume of data on its upper layer multiplied by the scaling factor. Valid values: 10 to 100. The number of layers decreases with the value of `scaling_factor`. We recommend that you set this parameter to 50.|50|

|Parameter|Description|Default value|
|---------|-----------|-------------|
|`ef`|The number of the nearest neighbors that are scanned during an online search. A large value increases the recall ratio but slows down the search. Valid values: 100 to 1000.|100|

Sample request:

```
GET test/_search
{
  "query": {
    "hnsw": {
      "feature": {
        "vector": [1.5, 2.5],
        "size": 10,
        "ef": 100       
      }
    }
  }
}
```

|Parameter|Description|Default value|
|---------|-----------|-------------|
|`indices.breaker.vector.native.indexing.limit`|If the off-heap memory usage exceeds the value specified by this parameter, write operations are suspended. After Elasticsearch creates indexes and releases the memory, it resumes the write operations. If the circuit breaker is triggered, the system memory consumption is high. We recommend that you throttle the write throughput. If you are a beginner, we recommend that you use the default value.|70%|
|`indices.breaker.vector.native.total.limit`|The maximum proportion of off-heap memory used to create vector indexes. If the actual off-heap memory usage exceeds the value specified by this parameter, Elasticsearch may reallocate the shards. If you are a beginner, we recommend that you use the default value.|80%|

## FAQ

-   Q: How do I evaluate the recall ratio of documents?

    A: You can create two indexes. One uses the HNSW algorithm and the other uses the Linear Search algorithm. Keep other index settings consistent for the two indexes. Add the same vector data to the indexes from your client and refresh the indexes. Compare the document IDs returned by the HNSW index and the Linear Search index after the same query vector is used. Then, find out the same document IDs that are returned by both indexes.

    **Note:** Divide the number of document IDs returned by both indexes by the total number of returned document IDs to calculate the recall ratio of the documents.

-   Q: How do I resolve a circuitBreakingException error when I write data to Elasticsearch?

    A: This error indicates that the off-heap memory usage exceeds the proportion specified by the `indices.breaker.vector.native.indexing.limit` parameter and that the write operation is suspended. The default proportion is 70%. In most cases, after Elasticsearch creates indexes and releases memory, the write operation is automatically resumed. We recommend that you add a retry mechanism to the data write script on your client.

-   Q: Why is the CPU still working after the write operation is suspended?

    A: Elasticsearch creates vector indexes during both the refresh and flush processes. The vector index creation task may be still running even if the write operation is suspended. Computing resources are released after the final refresh is complete.


