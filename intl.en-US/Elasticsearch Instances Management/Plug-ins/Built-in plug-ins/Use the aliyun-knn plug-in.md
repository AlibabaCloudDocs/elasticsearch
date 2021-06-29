---
keyword: [aliyun-knn, vector search plug-in]
---

# Use the aliyun-knn plug-in

The aliyun-knn plug-in is a vector search engine developed by the Alibaba Cloud Elasticsearch team. It uses the vector library of Proxima, a vector search engine designed by Alibaba DAMO Academy. This plug-in can meet your requirements on vector searches in various scenarios, such as image search, video fingerprinting, facial and speech recognition, and commodity recommendation. This topic describes how to use the aliyun-knn plug-in.

-   The aliyun-knn plug-in is installed. Whether this plug-in is installed by default is determined by your Elasticsearch cluster version and kernel version.

    |Cluster version|Kernel version|Description|
    |---------------|--------------|-----------|
    |V6.7.0|Earlier than V1.2.0|    -   You must manually install the aliyun-knn plug-in on the Plug-ins page of the Elasticsearch console based on the instructions provided in [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md).
    -   You are not allowed to use the script query and index warm-up features of the aliyun-knn plug-in. |
    |V6.8|None|
    |V7.4|None|
    |V7.7|None|
    |V6.7.0|V1.2.0 or later|    -   The aliyun-knn plug-in is integrated into the apack plug-in, which is installed by default. If you want to remove or reinstall the aliyun-knn plug-in, you must perform operations on the apack plug-in. For more information, see [Use the physical replication feature of the apack plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md).
    -   You can use the script query and index warm-up features of the aliyun-knn plug-in. Before you use the features, make sure that the version of the apack plug-in installed on your cluster is V1.2.1 or later. You can run the GET \_cat/plugins?v command to obtain the version of the apack plug-in. If the version of the apack plug-in is earlier than V1.2.1, you can [submit a ticket](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A//ticket-intl.console.aliyun.com/%23) to ask Alibaba Cloud engineers to update the plug-in. |
    |V7.10.0|V1.4.0 or later|    -   The aliyun-knn plug-in is integrated into the apack plug-in, which is installed by default. If you want to remove or reinstall the aliyun-knn plug-in, you must perform operations on the apack plug-in. For more information, see [Use the physical replication feature of the apack plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md).
    -   If the kernel version of your cluster is V1.4.0 or later, the version of the apack plug-in is the latest. You can run the GET \_cat/plugins?v command to obtain the version of the apack plug-in. |
    |Other versions|None|The vector search feature is not supported.|

    **Note:**

    -   The [kernel version](/intl.en-US/AliES Kernel/AliES release notes.md) is different from the version of the apack plug-in. You can run the GET\_cat /plugins?v command to obtain the version of the apack plug-in.
    -   Before you install the aliyun-knn plug-in, make sure that the data node specifications of your cluster are 2 vCPUs and 8 GiB of memory or higher. The specifications of 2 vCPUs and 8 GiB of memory are used only for functional tests. In a production environment, the data node specifications of your cluster must be 4 vCPUs and 16 GiB of memory or higher. If the data node specifications of your cluster do not meet these requirements, upgrade the data nodes in your cluster. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).
-   Your cluster and the indexes that you want to store on the cluster are planned. For more information, see [Index planning](#section_x17_tbz_22y) and [Cluster planning](#section_ae0_2bt_ki4).

-   Use scenario

    The vector search engine of Alibaba Cloud Elasticsearch is used in numerous production scenarios inside Alibaba Group, such as Pailitao, Image Search, Youku video fingerprinting, Qutoutiao video fingerprinting, Taobao commodity recommendation, customized searches, and Crossmedia searches.

-   Principle

    The vector search feature of Alibaba Cloud Elasticsearch is implemented based on the aliyun-knn plug-in. The plug-in is compatible with all the versions of open source Elasticsearch. Therefore, you can use the plug-in without additional learning costs. In addition to real-time incremental synchronization and near-real-time \(NRT\) searches, vector indexes support the other features of open source Elasticsearch in distributed searches. The features include multiple replica shards, restoration, and snapshots.

-   Algorithms

    The aliyun-knn plug-in supports the Hierarchical Navigable Small World \(HNSW\) and Linear Search algorithms. These algorithms are suitable for processing small amounts of data from in-memory storage. The following description compares the performance of the algorithms:

    |Performance metric|HNSW|Linear Search|
    |------------------|----|-------------|
    |Top-10 recall ratio|98.6%|100%|
    |Top-50 recall ratio|97.9%|100%|
    |Top-100 recall ratio|97.4%|100%|
    |Latency \(p99\)|0.093s|0.934s|
    |Latency \(p90\)|0.018s|0.305s|

    **Note:** p is short for percentage. For example, latency \(p99\) indicates the number of seconds that are required to respond to 99% of queries.


## Index planning

|Algorithm|Use scenario|In-memory storage|Remarks|
|---------|------------|-----------------|-------|
|HNSW|-   Each node stores only small volumes of data.
-   A low response latency is required.
-   A high recall ratio is required.

|Yes|-   HNSW is based on the greedy search algorithm and obeys triangle inequality. Triangle inequality states that the total sum of the costs from A to B and the costs from B to C must be greater than the costs from A to C. Inner product spaces do not obey triangle inequality. In this case, you must convert them to Euclidean spaces or spherical spaces before you use the HNSW algorithm.
-   After you write data to your Alibaba Cloud Elasticsearch cluster, we recommend that you call the force merge API on a regular basis during off-peak hours to merge the segments in shards. This reduces the response latency. |
|Linear Search|-   Brute-force searches need to be performed.
-   A recall ratio of 100% is required.
-   Latency increases with the volume of the data that needs to be processed.
-   Effect comparison is required.

|Yes|None.|

## Cluster planning

|Item|Description|
|----|-----------|
|Data node specifications \(required\)|The minimum specifications of a data node in a production environment must be 4 vCPUs and 16 GiB of memory. The specifications of 2 vCPUs and 8 GiB of memory are used only for functional tests.|
|Maximum volume of the data that can be stored on each data node|The maximum volume of the data that can be stored on each data node must be 50% of the total memory size of the data node.|
|Write throttling|Vector indexing is a CPU-intensive job. We recommend that you do not maintain a high write throughput. A peak write throughput lower than 5,000 transactions per second \(TPS\) is recommended for a data node with 16 vCPUs and 64 GiB of memory. When the system processes queries on vector indexes, it loads all the indexes to node memory. If nodes are out of memory, the system reallocates shards. Therefore, we recommend that you do not write large amounts of data to your cluster when the system is processing queries. |

## Create a vector index

1.  Log on to the Kibana console of your Alibaba Cloud Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to create a vector index:

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
              "dim": 2, 
              "vector_type": "float", 
              "distance_method": "SquaredEuclidean" 
            }
          }
        }
      }
    }
    ```

    |Category|Parameter|Default value|Description|
    |--------|---------|-------------|-----------|
    |setting|index.codec|proxima|Specifies whether to create the `proxima` vector index. Valid values:    -   proxima: The system creates the `proxima` vector index. This index supports vector searches. We recommend that you set this parameter to proxima.
    -   null: The system does not create the `proxima` vector index but creates only forward indexes. In this case, fields of the `proxima_vector` type do not support HNSW- or Linear Search-based queries. These fields support only script queries.
**Note:** If your cluster stores large volumes of data and you do not have high requirements on the latency of queries, you can remove the parameter or set the parameter to null. In this case, you can use the script query feature to search for the k-nearest neighbors \(k-NN\) of a vector. This feature is available only for V6.7.0 clusters whose apack plug-in is of V1.2.1 or later and V7.10.0 clusters whose apack plug-in is of V1.4.0 or later. |
    |index.vector.algorithm|HNSW|The algorithm that is used to search for vectors. Valid values:    -   hnsw: HNSW
    -   linear: Linear Search |
    |index.vector.general.builder.offline\_mode|false|Specifies whether to use the offline optimization mode to create the vector index. Valid values:    -   false: The offline optimization mode is not used.
    -   true: The offline optimization mode is used. In this case, the number of segments that are written to the vector index is significantly reduced. This improves write throughput.
**Note:**

    -   The offline optimization mode is available only for V6.7.0 clusters whose apack plug-in is of V1.2.1 or later and V7.10.0 clusters whose apack plug-in is of V1.4.0 or later. You are not allowed to perform script queries on the indexes for which the offline optimization mode is used.
    -   If you want to write all of your data to your cluster at a time, we recommend that you use the offline optimization mode. |
    |mapping|type|proxima\_vector|The field type. For example, if you set the type parameter to `proxima_vector` for the `feature` field, the `feature` field is a vector-type field.|
    |dim|2|The number of vector dimensions. Valid values: 1 to 2048.|
    |vector\_type|float|The data type of vectors. Valid values:    -   float
    -   short
    -   binary
If you set this parameter to binary, the vector data that you want to write to the vector index must be of the uint32 data type. This indicates that the vector data must be represented by an unsigned 32-bit decimal array. In addition, the value of the `dim` parameter must be a multiple of 32.

For example, if you want to write the 64-bit binary business data 0001 0010 0100 1001 0011 0101 1001 0111 1001 0010 0100 1001 0011 0101 1001 0110 to the `vector` field in your vector index, specify `"vector": [-1994006369, 1128900228]` in the command that is used to add documents.

**Note:** Only V6.7.0 clusters whose apack plug-in is of V1.2.1 or later and V7.10.0 clusters whose apack plug-in is of V1.4.0 or later support all the preceding three data types. Other clusters support only the float data type. |
    |distance\_method|SquaredEuclidean|The function that is used to calculate the distance between vectors. Valid values:    -   SquaredEuclidean: calculates the Euclidean distance without square root extraction.
    -   InnerProduct: calculates the inner product.
    -   Cosine: calculates the cosine similarity.
    -   Hamming: calculates the Hamming distance. This function is available only when you set the vector\_type parameter to binary.
**Note:**

    -   Only V6.7.0 clusters whose apack plug-in is of V1.2.1 or later and V7.10.0 clusters whose apack plug-in is of V1.4.0 or later support all the preceding four functions. Other clusters support only the SquaredEuclidean function.
    -   For more information about the preceding functions, see [Distance measurement functions](#section_xzj_9xg_b0a). |

    **Note:**

    -   You can run the GET \_cat/plugins?v command to obtain the version of the apack plug-in. If the version of the apack plug-in does not meet the requirements, you can [submit a ticket](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A//ticket-intl.console.aliyun.com/%23) to ask Alibaba Cloud engineers to update your apack plug-in.
    -   The aliyun-knn plug-in also supports some advanced search parameters. For more information, see [Advanced parameters](#section_76w_ddt_s0c).
4.  Run the following command to add a document:

    ```
    POST test/_doc
    {
      "feature": [1.0, 2.0], 
      "id": 1
    }
    ```

    **Note:** If you set the vector\_type parameter to binary when you create the vector index, the vector data that you want to write to the vector index must be of the uint32 data type. This indicates that you must convert your data to an unsigned 32-bit decimal array before you add a document. In addition, the value of the `dim` parameter must be a multiple of 32. If you set the vector\_type parameter to a value other than binary, the length of your vector data must be the same as the value specified for the `dim` parameter.


## Search for a vector

-   Standard search

    Run the following command to perform a standard search for a vector:

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

    The following table describes the parameters in the preceding command.

    |Parameter|Description|
    |---------|-----------|
    |hnsw|The algorithm that is used to search for the vector. The value must be the same as that of the `algorithm` parameter specified when you create the index.|
    |vector|The vector for which you want to search. The length of the array for the vector must be the same as the value of the `dim` parameter specified in `mapping`.|
    |size|The number of recalled documents. **Note:** The size parameter in a vector search command is different from the size parameter provided by Elasticsearch. The former controls the number of documents recalled by the aliyun-knn plug-in, whereas the latter controls the number of documents recalled for a search. When you perform a vector search, the system recalls the top N documents based on the value of the size parameter in the related vector search command and recalls all the matching documents based on the value of the size parameter provided by Elasticsearch. Then, the system returns results based on the top N documents and all the matching documents.

We recommend that you set the two size parameters to the same value. The default value of the size parameter provided by Elasticsearch is 10. |

    **Note:** The aliyun-knn plug-in also supports some advanced search parameters. For more information, see [Advanced parameters](#section_76w_ddt_s0c).

-   Script query

    Script queries are compatible with Elastic domain-specific language \(DSL\) queries. For example, you can use the [script\_score](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-script-score-query.html#vector-functions) parameter to score each document in a query response. The score is calculated by using the following formula: `1/(1 + l2Squared(params.queryVector, doc['feature']))`. The following code provides a sample script query:

    ```
    GET test/_search
    {
      "query": {
        "match_all": {}
      },
      "rescore": {
        "query": {
          "rescore_query": {
            "function_score": {
              "functions": [{
                "script_score": {
                  "script": {
                      "source": "1/(1+l2Squared(params.queryVector, doc['feature'])) ", 
                      "params": {
                        "queryVector": [2.0, 2.0]
                      }
                  }
                }
              }]
            }
          }
        }
      }
    }
    ```

    In addition to the functions supported by script queries in open source Elasticsearch, the script queries in Alibaba Cloud Elasticsearch support the functions described in the following table.

    |Function|Description|
    |--------|-----------|
    |l2Squared\(float\[\] queryVector, DocValues docValues\)|The function that is used to search for a vector based on a Euclidean distance.|
    |hamming\(float\[\] queryVector, DocValues docValues\)|The function that is used to search for a vector based on a Hamming distance.|
    |cosineSimilarity\(float\[\] queryVector, DocValues docValues\)|The function that is used to search for a vector based on a cosine similarity.|

    **Note:**

    -   You can perform script queries only in V6.7.0 clusters whose apack plug-in is of V1.2.1 or later and V7.10.0 clusters whose apack plug-in is of V1.4.0 or later. You can run the GET \_cat/plugins?v command to obtain the version of the apack plug-in. If the version of the apack plug-in does not meet the requirements, you can [submit a ticket](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A//ticket-intl.console.aliyun.com/%23) to ask Alibaba Cloud engineers to update your apack plug-in.
    -   Parameters in the preceding functions:
        -   float\[\] queryVector: the query vector. You can set this parameter to a formal or actual parameter.
        -   DocValues docValues: the document vectors.
    -   You are not allowed to perform script queries on the indexes that are created by using the offline optimization mode.
-   Index warm-up

    Searches are performed on vector indexes in in-memory storage. Therefore, after you write data to a vector index, the latency of the first search is high. This is because the index is still being loaded to memory. To address this issue, the aliyun-knn plug-in provides the index warm-up feature. This feature allows you to warm up your vector indexes and load them to the memory of your on-premises machine before the aliyun-knn plug-in provides vector search services. This significantly reduces the latency of vector searches.

    -   Run the following command to warm up all vector indexes:

        ```
        POST _vector/warmup
        ```

    -   Run the following command to warm up a specific vector index:

        ```
        POST _vector/{indexName}/warmup
        ```

    **Note:**

    -   The index warm-up feature is available only for V6.7.0 clusters whose apack plug-in is of V1.2.1 or later and V7.10.0 clusters whose apack plug-in is of V1.4.0 or later. You can run the GET \_cat/plugins?v command to obtain the version of the apack plug-in. If the version of the apack plug-in does not meet the requirements, you can [submit a ticket](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A//ticket-intl.console.aliyun.com/%23) to ask Alibaba Cloud engineers to update your apack plug-in.
    -   If your cluster stores a large number of vector indexes, the indexes store large volumes of data, and vector searches are required only for a specific vector index, we recommend that you warm up only the specific vector index to improve the search performance.

## Vector scoring

A unified scoring formula is provided for vector searches. The formula depends on [distance measurement functions](#section_xzj_9xg_b0a). The distance measurement functions affect the sorting of search results.

Scoring formula:

Score = 1/\(Vector distance + 1\)

**Note:**

-   By default, the scoring mechanism uses Euclidean distances without square root extraction to calculate scores.
-   In practice, you can calculate a distance based on a score. Then, optimize vectors based on the distance to increase the score.

## Distance measurement functions

Different distance measurement functions correspond to different scoring mechanisms. The following table describes the distance measurement functions supported by the aliyun-knn plug-in.

|Distance measurement function|Description|Scoring formula|Use scenario|Example|
|-----------------------------|-----------|---------------|------------|-------|
|SquaredEuclidean|This function is used to calculate the [Euclidean distance](https://baike.baidu.com/item/%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E5%BA%A6%E9%87%8F/1274107?fromtitle=%E6%AC%A7%E5%BC%8F%E8%B7%9D%E7%A6%BB&fromid=2809635&fr=aladdin) between vectors. The Euclidean distance, also called Euclidean metric, refers to the actual distance between points or the natural length of a vector in an m-dimensional space. Euclidean distances are widely used. A Euclidean distance in a two- or three-dimensional space is the actual distance between points.|For example, two n-dimensional vectors \[A1, A2, ..., An\] and \[B1, B2, ..., Bn\] exist.-   Euclidean distance without square root extraction = \(A1 - B1\)² + \(A2 - B2\)² + ... + \(An - Bn\)²
-   Score = 1/\(Euclidean distance + 1\)

**Note:** By default, the scoring mechanism uses Euclidean distances without square root extraction to calculate scores.

|A Euclidean distance can reflect the absolute difference between the characteristics of individual numbers. Therefore, Euclidean distances are widely used to analyze the differences between numbers from various dimensions. For example, you can use a user behavior metric to analyze the differences or similarities between user values.|Two two-dimensional vectors \[0,0\] and \[1,2\] exist. In this case, the Euclidean distance without square root extraction between the vectors is 5, which is calculated by using the following formula: \(1 - 0\)² + \(2 - 0\)².|
|Cosine|This function is used to calculate the [cosine similarity](https://baike.baidu.com/item/%E4%BD%99%E5%BC%A6%E7%9B%B8%E4%BC%BC%E5%BA%A6/17509249?fr=aladdin) between vectors. The function calculates the cosine of the angle between the vectors to obtain the cosine similarity between them.|For example, two n-dimensional vectors \[A1, A2, ..., An\] and \[B1, B2, ..., Bn\] exist.-   ![Cosine](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9290802261/p267586.png)

-   Score = 1/\(Cosine similarity + 1\)

|A cosine similarity reflects the differences between vectors from orientations. Cosine similarities are used to score content to obtain the similarities or differences between user interests. In addition, cosine similarities are subject to the orientations of vectors rather than the numbers in them. Therefore, the cosine similarities can address the issue that users may use different measurement standards.|For example, two two-dimensional vectors \[1,1\] and \[1, 0\] exist. The cosine similarity between the vectors is 0.707.|
|InnerProduct|This function is used to calculate the inner product between vectors. An inner product is also called a [dot product](https://baike.baidu.com/item/%E7%82%B9%E7%A7%AF/9648528?fromtitle=%E5%86%85%E7%A7%AF&fromid=422863). A dot product is a dyadic operation that takes two [vectors](https://baike.baidu.com/item/%E5%90%91%E9%87%8F/1396519) of the [real number](https://baike.baidu.com/item/%E5%AE%9E%E6%95%B0/296419) R and returns a scalar for the real number.|For example, two n-dimensional vectors \[A1, A2, ..., An\] and \[B1, B2, ..., Bn\] exist.-   Inner product between the vectors = A1B1 + A2B2 + ... + AnBn
-   Score = 1/\(Inner product + 1\)

|The inner product takes both the angle and absolute length between the vectors into consideration. After vectors are normalized, the formula for inner products is equivalent to that for cosine similarities.|For example, two two-dimensional vectors \[1,1\] and \[1,5\] exist. The inner product between the vectors is 6.|
|Hamming \(available only for vectors of the BINARY data type\)|In information theory, the [Hamming distance](https://baike.baidu.com/item/%E6%B1%89%E6%98%8E%E8%B7%9D%E7%A6%BB/475174?fr=aladdin) between strings of the same length equals the number of positions at which symbols differ. d\(x, y\) is used to represent the Hamming distance between the strings x and y. In other words, a Hamming distance measures the minimum number of substitutions that are required to change the string x into the string y.|For example, two n-bit binary strings x and y exist.-   d\(x,y\) = ∑x\[i\]⊕y\[i\]

In this formula, i is 0,1,..n-1, and ⊕ indicates exclusive or \(XOR\).

-   Score = 1/\(Hamming distance + 1\)

|Hamming distances are used to detect or fix the errors that occur when data is transmitted over computer networks. A Hamming distance can also be used as an error estimation method to determine the number of different characters between binary strings.|For example, the Hamming distance between 1011101 and 1001001 is 2. **Note:** When you use the aliyun-knn plug-in, the vector data that you want to write to your vector index must be of the uint32 data type. This indicates that the vector data must be represented by an unsigned 32-bit decimal array. In addition, the value of the `dim` parameter must be a multiple of 32. |

**Note:**

-   Only V6.7.0 clusters whose apack plug-in is of V1.2.1 or later and V7.10.0 clusters whose apack plug-in is of V1.4.0 or later support all the preceding four functions. Other clusters support only the SquaredEuclidean function.
-   You can run the GET \_cat/plugins?v command to obtain the version of the apack plug-in for your cluster. If the version of the apack plug-in does not meet the requirements, you can [submit a ticket](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A//ticket-intl.console.aliyun.com/%23) to ask Alibaba Cloud engineers to update the apack plug-in.
-   The distance\_method parameter in the `mapping` configuration is used to specify the distance measurement function that is used for your vector index.

## Circuit breaker parameters

|Parameter|Description|Default value|
|---------|-----------|-------------|
|`indices.breaker.vector.native.indexing.limit`|If the off-heap memory usage exceeds the value specified by this parameter, write operations are suspended. After the system creates indexes and releases the memory, the system resumes the write operations. If the circuit breaker is triggered, the consumption of system memory is high. In this case, we recommend that you throttle the write throughput.|70%|
|`indices.breaker.vector.native.total.limit`|The maximum proportion of off-heap memory used to create vector indexes. If the actual off-heap memory usage exceeds the value specified by this parameter, the system may reallocate shards.|80%|

**Note:** The preceding circuit breaker parameters are cluster-level parameters. You can run the GET \_cluster/settings command to view the values of the parameters. If you are a beginner, we recommend that you retain the default values of the parameters.

## Advanced parameters

|Parameter|Description|Default value|
|---------|-----------|-------------|
|`index.vector.hnsw.builder.max_scan_num`|The maximum number of the nearest neighbors that can be scanned when a graph is created under the worst case.|100000|
|`index.vector.hnsw.builder.neighbor_cnt`|The maximum number of the nearest neighbors that each node can have at layer 0. We recommend that you set this parameter to 100. The quality of a graph increases with the value of this parameter. However, inactive indexes consume more storage resources.|100|
|`index.vector.hnsw.builder.upper_neighbor_cnt`|The maximum number of the nearest neighbors that each node can have at a layer other than layer 0. We recommend that you set this parameter to 50% of the value specified for the `index.vector.hnsw.builder.neighbor_cnt` parameter.|50|
|`index.vector.hnsw.builder.efconstruction`|The number of the nearest neighbors that can be scanned when a graph is created. The quality of a graph increases with the value of this parameter. However, a longer time period is required to create indexes. We recommend that you set this parameter to 400.|400|
|`index.vector.hnsw.builder.max_level`|The total number of layers, which includes layer 0. For example, if you have 10 million documents and the `scaling_factor` parameter is set to 30, use 30 as the base number and round the logarithm of 10,000,000 up to the nearest integer. The result is 5. This parameter does not have a significant impact on vector searches. We recommend that you set this parameter to 6.

|6|
|`index.vector.hnsw.builder.scaling_factor`|A scaling factor. The volume of data at a layer equals the volume of data at its upper layer multiplied by the scaling factor. Valid values: 10 to 100. The number of layers decreases with the value of `scaling_factor`. We recommend that you set this parameter to 50.|50|

**Note:** You can set the preceding parameters in the `setting` configuration only after you set the index.vector.algorithm parameter to hnsw.

|Parameter|Description|Default value|
|---------|-----------|-------------|
|`ef`|The number of the nearest neighbors that are scanned during an online search. A large value increases the recall ratio but slows down searches. Valid values: 100 to 1000.|100|

The following code provides a sample search request:

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

## FAQ

-   Q: How do I evaluate the recall ratio of documents?

    A: You can create two indexes. One uses the HNSW algorithm and the other uses the Linear Search algorithm. Keep the other index settings consistent for the two indexes. Use a client to add the same vector data to the indexes, and refresh the indexes. Compare the document IDs returned by the HNSW index and the Linear Search index after the same query vector is used. Then, find out the same document IDs that are returned by both indexes.

    **Note:** Divide the number of document IDs returned by both indexes by the total number of returned document IDs to calculate the recall ratio of the documents.

-   Q: When I write data to my cluster, the system returns the "circuitBreakingException" error. What do I do?

    A: This error indicates that the off-heap memory usage exceeds the proportion specified by the `indices.breaker.vector.native.indexing.limit` parameter and that the write operation is suspended. The default proportion is 70%. In most cases, the write operation is automatically resumed after the system creates indexes and releases memory. We recommend that you add a retry mechanism to the data write script on your client.

-   Q: Why is the CPU still working after the write operation is suspended?

    A: The system creates vector indexes during both the refresh and flush processes. The index creation task may be still running even if the write operation is suspended. Computing resources are released after the final refresh is complete.

-   Q: Is the best practice for the aliyun-knn plug-in provided?

    A: The Alibaba Cloud developer community provides the [business scenarios](https://developer.aliyun.com/article/748117?spm=a2c6h.12873581.0.0.84736ec7wpDhiJ&groupCode=es) and [best practice](https://developer.aliyun.com/article/750481?spm=a2c6h.12873581.0.0.84736ec7wpDhiJ&groupCode=es) of the aliyun-knn plug-in.


