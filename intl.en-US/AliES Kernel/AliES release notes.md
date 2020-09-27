# AliES release notes

AliES is a highly tailored kernel for Alibaba Cloud Elasticsearch and supports all features provided by the open-source Elasticsearch kernel. AliES also provides additional features, such as monitoring metric optimization, thread pooling, circuit breaking optimization, and query and write performance optimization. These features not only improve cluster stability and data security but extend the scope of monitoring and O&M. This topic describes changes to AliES.

## Alibaba Cloud Elasticsearch V6.7.0 of the Standard Edition

|Kernel version|Description|
|--------------|-----------|
|1.2.0|-   The [physical replication](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md) feature is provided to improve the write performance of indexes that have replica shards.
-   The [pruning]() feature is supported for time series indexes. This feature improves the query performance of such indexes.
-   Primary key-based data deduplication is optimized during queries. This increases the write performance of documents that contain primary keys by 10%.
-   Finite state transducers \(FSTs\) that do not occupy heap memory are supported. A single node can store up to 20 TiB of index data. |
|1.0.2|The [access logs](/intl.en-US/Elasticsearch Instances Management/Query logs.md) of clusters can be viewed. These logs include fields such as Time, Node IP, and Content. You can use these logs to troubleshoot problems and analyze requests.|
|1.0.1|JVM circuit breaking policies can be configured. After the usage of JVM heap memory reaches 95%, the system rejects requests to protect clusters. The following parameters are used to configure the policies: -   indices.breaker.total.use\_real\_memory: The default value is false.
-   indices.breaker.total.limit: The default value is 95%. |
|0.3.0|-   [The scheduling performance of dedicated master nodes is improved by 10 times](https://developer.aliyun.com/article/745572?utm_content=g_1000115954). Each dedicated master node is allowed to schedule more shards.
-   Write performance is improved by 10%, and the overheads incurred by translog flush are reduced. |

