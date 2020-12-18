# AliES release notes

AliES is a highly tailored kernel for Alibaba Cloud Elasticsearch and supports all features provided by the open source Elasticsearch kernel. AliES also provides additional features, such as monitoring metric optimization, thread pooling and circuit breaking optimization, and query and write performance optimization. These additional features are developed based on the abundant experience of the Alibaba Cloud Elasticsearch team from multiple scenarios. These features not only improve cluster stability and performance but also reduce costs and extend the scope of monitoring and O&M. This topic describes the optimized features in each version of AliES.

## Alibaba Cloud Elasticsearch V6.7.0 of the Standard Edition

|Kernel version|Description|
|--------------|-----------|
|1.3.0|-   The [slow query isolation](/intl.en-US/AliES Kernel/Use the slow query isolation feature.md) feature is provided to reduce the impact of anomalous queries on cluster stability.
-   The [gig plug-in]() is provided to perform a switchover within seconds when exceptions occur in a cluster. This plug-in reduces the probability that an anomalous node causes query jitters.

**Note:** Before you use the preceding features, make sure that the kernel version of your Elasticsearch cluster is 1.3.0. Otherwise, upgrade the kernel. You can upgrade only the kernels of Standard Edition clusters whose kernel versions are V0.3.0, V1.0.2, or V1.3.0. |
|1.2.0|-   The [physical replication](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md) feature is provided to improve the write performance of indexes that have replica shards.
-   The [pruning](/intl.en-US/AliES Kernel/Use the pruning feature for a time series index.md) feature is supported for time series indexes. This feature improves the query performance of such indexes.
-   Primary key-based data deduplication is optimized during queries. This improves the write performance of documents that contain primary keys by 10%.
-   Finite state transducers \(FSTs\) that do not occupy heap memory are supported. A single node can store a maximum of 20 TiB of index data. |
|1.0.2|The [access logs](/intl.en-US/Elasticsearch Instances Management/Query logs.md) of clusters can be viewed. These logs include fields such as Time, Node IP, and Content. You can use these logs to troubleshoot problems and analyze requests.|
|1.0.1|JVM circuit breaking policies can be configured. After the usage of JVM heap memory reaches 95%, the system rejects requests to protect clusters. The following parameters are used to configure the policies: -   indices.breaker.total.use\_real\_memory: The default value is false.
-   indices.breaker.total.limit: The default value is 95%. |
|0.3.0|-   [The scheduling performance of dedicated master nodes is improved by 10 times](https://developer.aliyun.com/article/745572?utm_content=g_1000115954). Each dedicated master node is allowed to schedule more shards.
-   Write performance is improved by 10%, and the overheads of translog flush are reduced. |

