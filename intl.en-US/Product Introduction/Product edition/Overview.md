# Overview

Alibaba Cloud Elasticsearch offers two editions: Standard Edition and Advanced Edition. The two editions support different versions. This topic describes how to view the edition and version of an Elasticsearch cluster and provides comparisons between various editions and versions.

## View the edition and version of a cluster

You can view the edition and version of your cluster on the **Basic Information** page of the cluster in the Elasticsearch console. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).

## Edition comparisons

|Item|Standard Edition|Advanced Edition|
|----|----------------|----------------|
|Features|-   All the open source features are supported.
-   Multiple open source versions are provided.
-   The AliES kernel is provided and is optimized in minor versions.
-   A variety of capabilities are provided to meet requirements in various scenarios.

|-   The compute-storage separation architecture is used.
-   Open source features are supported, and the kernel is optimized for logging scenarios.
-   High write throughput and low storage costs are provided.
-   A shared storage mechanism is used to achieve auto scaling within seconds. |
|Scenarios|All scenarios are supported, such as log analysis, searches, and data analysis.|The following scenarios are supported:

-   Scenarios where large amounts of logs need to be analyzed
-   Scenarios where highly concurrent queries exist |
|User personas|-   Users who want to use Elasticsearch in multiple scenarios.
-   Users who have planned their resources.
-   Users who understand Elasticsearch, Logstash, and Kibana \(ELK\) and have the ability to optimize cluster performance in various scenarios.

|-   Users who want to reduce the costs in logging scenarios.
-   Users who want to reduce the O&M costs caused by business fluctuations.
-   Users who have high requirements on optimizing write performance. |
|Supported versions|V5.5, V5.6, V6.3, V6.7, V6.8, V7.4, and V7.7 are supported.|Only V6.7 is supported.|
|Billing mode|You are charged for your cluster based on the specifications, storage, and number of nodes in the cluster.|You are charged for your cluster based on the specifications and number of nodes in the cluster and the storage used by hot data.|

## Version comparisons

|Open source Elasticsearch version|Alibaba Cloud Elasticsearch version|Feature change|
|---------------------------------|-----------------------------------|--------------|
|5.x|V5.5 and V5.6|-   An index can have multiple types, and custom types are supported.
-   The STRING data type is replaced by the TEXT or KEYWORD data type.
-   The values of fields in indexes are changed from not\_analyzed or no to true or false.
-   The DOUBLE data type is replaced by the FLOAT data type to reduce storage costs.
-   Java High Level REST Client is launched to replace Transport Client.

For more information, see [Breaking changes in 5.0](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/breaking-changes-5.0.html). |
|6.x|V6.3, V6.7, and V6.8|-   An index can have only one type, and the \_doc type is recommended.
-   The index lifecycle management \(ILM\) feature is introduced from V6.6.0 to reduce index O&M costs.
-   The [historical data rollup](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/rollup-overview.html) feature is introduced to help summarize historical data.
-   [Elasticsearch SQL](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/sql-overview.html), an X-Pack component, is supported from V6.3. It enables SQL statements to be converted to domain-specific language \(DSL\) statements. This reduces costs for learning DSL.
-   Some aggregation functions are added, such as [Composite](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-composite-aggregation.html), [Parent](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-parent-aggregation.html), and [Weighted Avg](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-metrics-weight-avg-aggregation.html).

For more information, see [Breaking changes in 6.0](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/breaking-changes-6.0.html). |
|7.x|V7.4 and V7.7|-   The default number of shards in the index template is changed from 5 to 1.
-   Mapping types are removed. When you define a mapping or an index template, you do not need to specify a mapping type. For more information, see [Removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/removal-of-types.html#_what_are_mapping_types).
-   By default, a maximum of 10,000 documents can be returned for each request. If more than 10,000 matching documents exist, Elasticsearch returns only 10,000 matching documents. For more information, see [track\_total\_hits 10000 default](https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking-changes-7.0.html#track-total-hits-10000-default).
-   By default, a single data node can store a maximum of 1,000 shards. You can use the cluster.max\_shards\_per\_node parameter to change the limit. For more information, see [Cluster Shard Limit](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/misc-cluster.html#cluster-shard-limit).
-   By default, a maximum of 500 scrolls can be performed. You can use the search.max\_open\_scroll\_context parameter to change the limit. For more information, see [Scroll Search Context](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/search-request-body.html#scroll-search-context).
-   The parent circuit breaker works based on the current memory usage. This is controlled by the indices.breaker.total.use\_real\_memory parameter. By default, the parent circuit breaker starts to work when the current memory usage is 95% of JVM heap memory usage. This indicates that Elasticsearch uses maximum memory availability to avoid out of memory \(OOM\) issues. For more information, see [Circuit Breaker](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/circuit-breaker.html#parent-circuit-breaker).
-   The \_all field is removed to improve search performance.
-   [Intervals queries](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/query-dsl-intervals-query.html) are supported. Elasticsearch searches for and returns documents based on the order and proximity of matching terms.
-   After the audit logging feature is enabled, audit events are persisted to <clustername\>\_audit.json in the file system of each node. The audit events cannot be stored in indexes. For more information, see [Enabling audit logging](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/enable-audit-logging.html).

For more information, see [Breaking changes in 7.0](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/breaking-changes-7.0.html#breaking_70_indices_changes). |

## Create an Elasticsearch cluster

-   [t134282.md\#](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md)
-   [Buy page parameters]()
-   [Buy page parameters \(Advanced Edition\)]()

