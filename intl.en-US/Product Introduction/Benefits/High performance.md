# High performance

Developed based on open source Elasticsearch, Alibaba Cloud Elasticsearch provides various features to optimize write and query performance in different scenarios. The features also help you reduce costs. This topic describes the features that are provided by Alibaba Cloud Elasticsearch to achieve high performance.

## High-performance hardware and high-speed access

Alibaba Cloud Elasticsearch supports a variety of servers and storage hardware, and follows the latest hardware iterations to fully ensure cluster performance and stability at the hardware level. In addition, communications over internal networks are used to reduce the response time of applications.

## Scenario-based templates

Alibaba Cloud Elasticsearch provides scenario-based templates. All parameters in the templates are developed and optimized based on years of experience. You can select an appropriate template based on your business requirements to optimize the read and write performance of your Elasticsearch cluster in the related scenario. This reduces cluster performance issues caused by inappropriate configurations.

## Kernel performance optimization

The AliES kernel of Alibaba Cloud Elasticsearch is developed and optimized to enhance cluster performance. Alibaba Cloud Elasticsearch allows you to update the kernel for higher cluster performance. For more information about the release notes of the AliES kernel, see [AliES release notes](/intl.en-US/AliES Kernel/AliES release notes.md).

Alibaba Cloud Elasticsearch provides the following high-performance features:

-   Physical replication: This feature improves the write performance of indexes that have replica shards. For more information, see [Use the physical replication feature of the apack plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md).
-   Pruning for time series indexes: This feature improves the query performance of time series indexes. For more information, see [Use the pruning feature for a time series index](/intl.en-US/AliES Kernel/Use the pruning feature for a time series index.md).
-   Primary key-based data deduplication during queries: This feature is optimized. It improves the write performance by 10% for documents that contain primary keys.
-   Shard scheduling by dedicated master nodes: This feature is improved. It improves the shard scheduling performance of dedicated master nodes by 10 times. Each dedicated master node is allowed to schedule more shards.
-   Translog optimization: This feature reduces the overheads of translog flush and improves write performance by 10%.

