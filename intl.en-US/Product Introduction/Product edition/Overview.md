# Overview

Alibaba Cloud Elasticsearch offers two editions: Standard Edition and Advanced Edition. The two editions support different cluster versions. This topic describes how to view the edition and version of an Elasticsearch cluster, compares the two editions, and compares the various versions.

## View the edition and version of a cluster

You can view the edition and version of your cluster on the **Basic Information** page of the cluster in the Elasticsearch console. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).

![View the edition and version of a cluster](../images/p176053.png)

## Edition comparisons

|Item|Standard Edition|Advanced Edition|
|----|----------------|----------------|
|Features|-   All the features provided by open source Elasticsearch are supported.
-   Multiple open source Elasticsearch versions are provided.
-   The AliES kernel is provided and is optimized in minor versions.
-   A variety of capabilities are provided to meet the requirements in various scenarios.

|-   The compute-storage separation architecture is used.
-   The features provided by open source Elasticsearch are supported, and the kernel is optimized for logging scenarios.
-   High write throughput is supported, and storage costs are low.
-   A shared storage mechanism is used to achieve auto scaling within seconds. |
|Scenarios|All scenarios are supported, such as log analysis, searches, and data analysis.|The following scenarios are supported:-   Scenarios in which large amounts of logs need to be analyzed
-   Scenarios in which highly concurrent queries exist |
|User personas|-   Users who want to use Elasticsearch in multiple scenarios.
-   Users who have planned their resources.
-   Users who understand Elasticsearch, Logstash, and Kibana \(ELK\) and have the ability to optimize cluster performance in various scenarios.

|-   Users who want to reduce the costs in logging scenarios.
-   Users who want to reduce the operations and maintenance \(O&M\) costs caused by business fluctuations.
-   Users who have high requirements on write performance optimization. |
|Supported versions|V5.5, V5.6, V6.3, V6.7, V6.8, V7.4, V7.7, and V7.10 are supported.|Only V6.7 is supported.|
|Billing mode|You are charged based on the specifications, storage, and number of nodes in your cluster.|You are charged based on the specifications and number of nodes in your cluster and the storage used by hot data.|

## Version comparisons

|Open source Elasticsearch version|Alibaba Cloud Elasticsearch version|Feature change|
|---------------------------------|-----------------------------------|--------------|
|7.x|V7.10|Alibaba Cloud Elasticsearch:

-   Some features, such as [pruning for time series indexes](/intl.en-US/AliES Kernel/Use the pruning feature for a time series index.md) and [slow query isolation](/intl.en-US/AliES Kernel/Use the slow query isolation feature.md), are introduced based on the AliES kernel to improve query performance.
-   The [faster-bulk](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the faster-bulk plug-in.md) and [gig](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the gig plug-in.md) plug-ins are provided in addition to the plug-ins provided by open source Elasticsearch. The two plug-ins are used to improve cluster stability.

Open source Elasticsearch:

-   The compression of storage fields is improved, which reduces storage costs.
-   The [snapshot-based search](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/searchable-snapshots-api-mount-snapshot.html) feature is introduced. This feature allows you to search for data in the snapshots that are stored in low-cost storage objects without the need to restore the data in the snapshots.
-   [Event Query Language \(EQL\)](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/eql.html) is used to improve security.
-   The default value of [search.max\_buckets](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/search-settings.html#search-settings-max-buckets) is changed from 10000 to 65535.
-   Queries that are not case-sensitive are supported. To implement such queries, you must set the [case\_insensitive](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/query-dsl-regexp-query.html) parameter to true.

For more information about feature changes, see [Breaking changes in 7.10](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/migrating-7.10.html#breaking-changes-7.10). |
|V7.4 and V7.7|Open source Elasticsearch:-   The default number of shards in the index template is changed from 5 to 1.
-   Mapping types are removed. You do not need to specify a mapping type when you define a mapping or an index template. For more information, see [Removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/removal-of-types.html#_what_are_mapping_types).
-   By default, a maximum of 10,000 documents can be returned for each request. If more than 10,000 matching documents exist, Elasticsearch returns only 10,000 matching documents. For more information, see [track\_total\_hits 10000 default](https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking-changes-7.0.html#track-total-hits-10000-default).
-   By default, a single data node can store a maximum of 1,000 shards. You can use the cluster.max\_shards\_per\_node parameter to change this limit. For more information, see [Cluster Shard Limit](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/misc-cluster.html#cluster-shard-limit).
-   By default, a maximum of 500 scrolls can be performed. You can use the search.max\_open\_scroll\_context parameter to change this limit. For more information, see [Scroll Search Context](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/search-request-body.html#scroll-search-context).
-   The parent circuit breaker works based on the current memory usage. This is controlled by the indices.breaker.total.use\_real\_memory parameter. By default, the parent circuit breaker starts to work when the current memory usage reaches 95% of JVM heap memory usage. This indicates that Elasticsearch uses the maximum memory availability to avoid out of memory \(OOM\) issues. For more information, see [Circuit Breaker](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/circuit-breaker.html#parent-circuit-breaker).
-   The \_all field is removed to improve search performance.
-   [Intervals queries](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/query-dsl-intervals-query.html) are supported. Elasticsearch searches for and returns documents based on the order and proximity of matching terms.
-   After the audit logging feature is enabled, audit events are persisted to <Cluster name\>\_audit.json in the file system of each node. The audit events cannot be stored in indexes. For more information, see [Enabling audit logging](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/enable-audit-logging.html).

For more information about feature changes, see [Breaking changes in 7.0](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/breaking-changes-7.0.html#breaking_70_indices_changes). |
|6.x|V6.3, V6.7, and V6.8|Open source Elasticsearch:-   An index can have only one type, and the \_doc type is recommended.
-   The index lifecycle management \(ILM\) feature is introduced from V6.6.0 to reduce index O&M costs.
-   The [historical data rollup](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/rollup-overview.html) feature is introduced to help summarize historical data.
-   [Elasticsearch SQL](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/sql-overview.html), an X-Pack component, is supported in V6.3 and later. It enables SQL statements to be converted to domain-specific language \(DSL\) statements. This reduces costs for learning DSL.
-   The [Composite](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-composite-aggregation.html), [Parent](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-parent-aggregation.html), and [Weighted Avg](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-metrics-weight-avg-aggregation.html) aggregation functions are supported.

For more information about feature changes, see [Breaking changes in 6.0](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/breaking-changes-6.0.html). |
|V6.7|Alibaba Cloud Elasticsearch:-   Some features, such as [pruning for time series indexes](/intl.en-US/AliES Kernel/Use the pruning feature for a time series index.md) and [slow query isolation](/intl.en-US/AliES Kernel/Use the slow query isolation feature.md), are introduced based on the AliES kernel to improve query performance.
-   The [faster-bulk](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the faster-bulk plug-in.md) and [gig](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the gig plug-in.md) plug-ins are provided in addition to the plug-ins provided by open source Elasticsearch. The two plug-ins are used to improve cluster stability.
-   The [one-click index migration]() feature is introduced. This feature allows you to configure only a few settings to migrate your data to the cloud. The feature is easier to use than the reindex API.
-   The Advanced Monitoring and Alerting service is introduced. This service implements fine-grained monitoring and alerting, such as shard- or segment-level monitoring and alerting.
-   Upgrades from V6.3.2 to V6.7.0 are supported. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md). |
|5.x|V5.5 and V5.6|Open source Elasticsearch:-   An index can have multiple types, and custom types are supported.
-   The STRING data type is replaced by the TEXT or KEYWORD data type.
-   The values of fields in indexes are changed from not\_analyzed or no to true or false.
-   The DOUBLE data type is replaced by the FLOAT data type to reduce storage costs.
-   Java High Level REST Client is launched to replace Transport Client.

For more information about feature changes, see [Breaking changes in 5.0](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/breaking-changes-5.0.html). |

## Create an Elasticsearch cluster

-   [t134282.md\#](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md)
-   [Buy page parameters]()
-   [Buy page parameters \(Advanced Edition\)]()

