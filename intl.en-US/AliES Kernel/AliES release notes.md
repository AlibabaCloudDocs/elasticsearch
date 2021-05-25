# AliES release notes

AliES is a highly tailored kernel for Alibaba Cloud Elasticsearch. AliES supports all the features provided by the open source Elasticsearch kernel and provides additional features, such as metric optimization, thread pooling, circuit breaking optimization, and query and write performance optimization. These additional features are developed based on the abundant experience of the Alibaba Cloud Elasticsearch team in multiple scenarios. These features not only improve cluster stability and performance but also reduce costs and extend the scope of monitoring and operations and maintenance \(O&M\). This topic describes the optimized features in each version of AliES.

## Alibaba Cloud Elasticsearch V7.10.0 of the Standard Edition

|Kernel version|Description|
|--------------|-----------|
|V1.3.0|-   The [slow query isolation](/intl.en-US/AliES Kernel/Use the slow query isolation feature.md) feature is provided to reduce the impact of anomalous queries on cluster stability.
-   The [gig plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the gig plug-in.md) is provided to perform a switchover within seconds after an exception occurs on a cluster. This plug-in reduces the probability that an anomalous node causes query jitters.

**Note:** For Alibaba Cloud Elasticsearch V7.10.0 clusters of the Standard Edition, the gig plug-in is integrated into the aliyun-qos plug-in. The aliyun-qos plug-in is installed by default.

-   The [physical replication](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md) feature is provided to improve the write performance of indexes that have replica shards.
-   The [pruning](/intl.en-US/AliES Kernel/Use the pruning feature for a time series index.md) feature is provided for time series indexes to improve the query performance of the indexes.
-   The [access logs](/intl.en-US/Elasticsearch Instances Management/Query logs.md) of clusters can be viewed. These logs contain fields such as Time, Node IP, and Content. You can use these logs to troubleshoot issues and analyze requests.
-   [The scheduling performance of dedicated master nodes is improved by 10 times](https://developer.aliyun.com/article/745572?utm_content=g_1000115954). Each dedicated master node can schedule more shards. |

## Alibaba Cloud Elasticsearch V6.7.0 of the Standard Edition

|Kernel version|Description|
|--------------|-----------|
|V1.3.0|-   The [slow query isolation](/intl.en-US/AliES Kernel/Use the slow query isolation feature.md) feature is provided to reduce the impact of anomalous queries on cluster stability.
-   The [gig plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the gig plug-in.md) is provided to perform a switchover within seconds after an exception occurs on a cluster. This plug-in reduces the probability that an anomalous node causes query jitters.

**Note:** Before you use the preceding features, make sure that the kernel version of your Elasticsearch cluster is V1.3.0. Otherwise, upgrade the kernel. You can upgrade only the kernels of Standard Edition clusters whose kernel versions are V0.3.0, V1.0.2, or V1.3.0. |
|V1.2.0|-   The [physical replication](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md) feature is provided to improve the write performance of indexes that have replica shards.
-   The [pruning](/intl.en-US/AliES Kernel/Use the pruning feature for a time series index.md) feature is provided for time series indexes to improve the query performance of the indexes.
-   Primary key-based data deduplication is optimized during queries. This improves the write performance of documents that contain primary keys by 10%.
-   Finite state transducers \(FSTs\) that do not occupy heap memory are supported. A single node can store a maximum of 20 TiB of index data. |
|V1.0.2|The [access logs](/intl.en-US/Elasticsearch Instances Management/Query logs.md) of clusters can be viewed. These logs contain fields such as Time, Node IP, and Content. You can use these logs to troubleshoot issues and analyze requests.|
|V1.0.1|Circuit breaking policies can be configured for Java virtual machines \(JVMs\). After the usage of the JVM heap memory of your cluster reaches 95%, the system rejects requests to protect the cluster. The following parameters are used to configure the policies: -   indices.breaker.total.use\_real\_memory: The default value is false.
-   indices.breaker.total.limit: The default value is 95%. |
|V0.3.0|-   [The scheduling performance of dedicated master nodes is improved by 10 times](https://developer.aliyun.com/article/745572?utm_content=g_1000115954). Each dedicated master node can schedule more shards.
-   Write performance is improved by 10%, and the overheads of translog flush are reduced. |

