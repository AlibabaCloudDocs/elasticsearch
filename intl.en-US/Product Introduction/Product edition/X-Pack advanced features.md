# X-Pack advanced features

This topic provides a purchase guideline for X-Pack advanced features, describes the commonly used X-Pack advanced features, and compares the features offered by different editions.

## Overview

X-Pack advanced features are the commercial features developed by the open source Elasticsearch team based on the X-Pack commercial plug-in. The features include security, SQL plug-in, machine learning, alerting, and monitoring. These features enhance the service capabilities of open source Elasticsearch in terms of application development and O&M management.

Alibaba Cloud Elasticsearch provides editions that support the advanced features. You can purchase the features when you create a cluster. The following sections describe the detailed information of these features.

## Purchase guideline

You can log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home) and click Create on the Elasticsearch Clusters page to purchase X-Pack advanced features. Only the Standard Edition of Alibaba Cloud Elasticsearch supports the advanced features. The following table lists the related information of the Standard Edition.

|Item|Standard Edition|
|----|----------------|
|Whether X-Pack is included|Yes|
|Whether all X-Pack features are provided|Yes|

**Note:**

In addition to X-Pack advanced features, Alibaba Cloud Elasticsearch clusters of the Standard Edition provide features such as O&M management, security, plug-ins, and high availability. For more information, see [Alibaba Cloud Elasticsearch clusters of the Standard Edition](/intl.en-US/Product Introduction/Product edition/Alibaba Cloud Elasticsearch clusters of the Standard Edition.md).

## Feature description

This section describes only a few commonly used advanced features. For more information about all the X-Pack advanced features, see [Elastic Stack subscriptions](https://www.elastic.co/cn/subscriptions) and [X-Pack APIs](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/xpack-api.html).

**Note:** The X-Pack advanced features provided by Elastic Stack differ in various editions, such as FREE AND OPEN, GOLD, and PLATINUM. Alibaba Cloud subscribes to Elasticsearch of the PLATINUM edition.

|Feature|Description|
|-------|-----------|
|Security|Manages indexes and fields in a decentralized manner and strictly controls access permissions to improve data security.|
|Machine learning|Monitors data in real time, provides the auto alerting feature, and reports alerts.|
|Monitoring|Monitors objects, such as clusters, nodes, and indexes, in real time to improve development efficiency and reduce O&M costs.|
|SQL plug-in|-   Implements full-text searches and statistical analysis on Elasticsearch data based on traditional SQL databases.
-   Supports access methods such as CLI and REST. In the PLATINUM edition, the SQL plug-in also supports the Java Database Connectivity \(JDBC\) method.
-   Seamlessly integrates with original business systems, which reduces the costs for learning new techniques.

**Note:** In the FREE AND OPEN edition, other SQL plug-ins are integrated. For more information, see [elasticsearch-sql](https://github.com/NLPchina/elasticsearch-sql). |

## Feature comparisons between editions

This section compares some of the key X-Pack features between the FREE AND OPEN and PLATINUM editions. This helps you understand the differences between features in the FREE AND OPEN edition and those in the PLATINUM edition. Elasticsearch is rapidly developing. Therefore, features supported in different editions are constantly updated. For more information about the latest comparisons of open source Elasticsearch features among different editions, see [Elastic Stack subscriptions](https://www.elastic.co/cn/subscriptions).

The following table compares some of the key X-Pack features in the FREE AND OPEN and PLATINUM editions.

**Note:** In the following table, the symbols ✓, ○, and × are used to indicate the integrity of the features.

-   ✓: indicates that all the subfeatures of a feature are provided.
-   ○: indicates that only some of the subfeatures of a feature are provided.
-   ×: indicates that a feature is not provided.

|Module|Feature|FREE AND OPEN|PLATINUM|
|------|-------|-------------|--------|
|Elasticsearch|Scalability and resiliency|○|✓|
|Query and analysis|○|✓|
|Stack management|○|✓|
|Security|×|✓|
|Machine learning|×|✓|
|Watcher|×|✓|
|Kibana|Exploration and visualization|○|✓|
|Stack management|○|✓|
|Stack detection|×|✓|
|Kibana alerting|×|✓|
|Security|×|✓|
|Machine learning|×|✓|
|Beats|Data collection|○|✓|
|Data transmission|○|✓|
|Monitoring and management|×|✓|
|Module|○|✓|
|Logstash|Data collection|✓|✓|
|Data enrichment|✓|✓|
|Data transmission|✓|✓|
|Module|○|✓|
|Monitoring and management|×|✓|
|Elastic APM|APM server|✓|✓|
|APM agent|✓|✓|
|Kibana APM dashboard|✓|✓|
|APM UI|×|✓|
|Distributed tracing|×|✓|
|Integration of machine learning|×|✓|
|Elastic logs|Log shipper \(Filebeat\)|✓|✓|
|Common data source dashboard|✓|✓|
|Logs UI|×|✓|
|Elastic infrastructure|Metric shipper|✓|✓|
|Common data source dashboard|✓|✓|
|Infrastructure UI|×|✓|
|Elastic status monitoring|Status monitoring \(Heartbeat\)|✓|✓|
|Status dashboard in Kibana|✓|✓|
|Status monitoring UI|×|✓|

## Some features provided by open source Elasticsearch

The following table lists some features provided by open source Elasticsearch.

|Category|Subcategory|Feature|
|--------|-----------|-------|
|Management and operations|Scalability and resiliency|Clustering and high availability|
|Automatic node recovery|
|Automatic data rebalancing|
|Horizontal scalability|
|Rack awareness|
|Cross-cluster replication|
|Cross-data center replication|
|Monitoring|Full stack monitoring|
|Multi-stack monitoring|
|Configurable retention policy|
|Automatic alerts on stack issues|
|Management|Index lifecycle management|
|Data tiers|
|Frozen indexes|
|Snapshot creation and data restoration|
|Searchable snapshots|
|Source-only snapshots|
|Snapshot lifecycle management|
|Data rollup|
|Data streams|
|CLI tools|
|Upgrade assistant UI|
|Upgrade assistant APIs|
|User and role management|
|Transforms|
|Alerting|Highly available, scalable alerting|
|Notifications|
|Alerting UI|
|Stack security|Security settings|
|Encrypted communications|
|Support for encryption at rest|
|RBAC|
|Field- and document-level security|
|Audit logging|
|IP address filtering|
|Security realms|
|Single sign-on \(SSO\)|
|Third-party security integration|
|Clients|RESTful APIs|
|Language clients|
|Console|
|DSL|
|SQL|
|Event query language \(EQL\)|
|JDBC client|
|ODBC client|
|Data collection and enrichment|Data sources|Operating systems|
|Web servers and proxies|
|Data repositories and queues|
|Cloud services|
|Containers|
|Network data|
|Security data|
|Running status data|
|File import|
|Data enrichment|Processors|
|Analyzers|
|Tokenizers|
|Filters|
|Language analyzers|
|Grok|
|Field transformation|
|External lookups|
|Match enrich processor|
|Geo-match enrich processor|
|Modules and integrations|Clients and APIs|
|Beats|
|Community shippers|
|Logstash|
|Elasticsearch-Hadoop|
|Plug-ins and integrations|
|Data storage|Flexibility|Data types|
|Full-text searches|
|Document databases|
|Time series and analysis|
|Geospatial|
|Security|Support for encryption at rest|
|Field-level security|
|Management|Clustered indexes|
|Snapshot creation and data restoration|
|Index rollup|
|Search and analysis|Full-text searches|Inverted indexes|
|Cross-cluster searches|
|Relevance scoring|
|Query DSL|
|Asynchronous searches|
|Highlighters|
|Automatic completion|
|Spelling checks and corrections|
|Suggesters|
|Percolators|
|Query optimizer|
|Permissions-based search results|
|Query cancellation|
|Analytics|Aggregations|
|Graph searches|
|Threshold-based alerting|
|Machine learning|Inference|
|Forecasting on time series|
|Anomaly detection on time series|
|Alerting on anomalies|
|APM|APM server|
|APM agents|
|APM applications|
|Distributed tracing|
|Alerting|
|Service maps|
|Visualization|Dashboards|
|Canvas|
|Kibana Lens|
|Time Series Visual Builder \(TSVB\)|
|Graph analysis|
|Geospatial analysis|
|Container monitoring|
|Kibana plug-ins|
|Data import tutorial|
|Maps|Map layers|
|Custom area maps|
|GeoJSON upload|
|Elastic logs|Log shipper|
|Log dashboards|
|Detection on log rate anomalies|
|Elastic metrics|Metric shipper|
|Metric dashboards|
|Alerting|
|Uptime|Uptime monitoring|
|Uptime dashboards|
|Alerting|
|Certificate monitoring|
|Synthetic monitoring|
|Security analysis|Common Schema|
|Security analysis|
|Timeline events|
|Case management|
|Anomaly detection|

