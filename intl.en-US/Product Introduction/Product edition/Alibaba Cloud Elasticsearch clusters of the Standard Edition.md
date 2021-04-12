# Alibaba Cloud Elasticsearch clusters of the Standard Edition

Alibaba Cloud Elasticsearch offers two editions of clusters: Standard Edition and Advanced Edition. This topic provides an overview of Alibaba Cloud Elasticsearch clusters of the Standard Edition. Alibaba Cloud Elasticsearch clusters of the Standard Edition provide fully managed Elasticsearch services. Such clusters support all the features provided by open source Elasticsearch and all the advanced features provided by the X-Pack plug-in. Multiple versions from V5.X to V7.X are available for the clusters. In addition, a variety of capabilities are provided to facilitate cluster management, configuration, and O&M and ensure security and high availability.

## Features

|Category|Feature|Description|
|--------|-------|-----------|
|Cluster management and O&M|Cluster creation|Enables you to quickly create and configure a cluster based on your business requirements. Multiple types of nodes and disk types are available for the cluster.|
|Cluster upgrade|Enables you to quickly upgrade the version or update the kernel of a cluster.|
|Scaling|Allows you to configure auto scaling rules or manually perform scaling for nodes to flexibly cope with your business fluctuations.|
|Cluster topology visualization|-   Allows you to view the status of a cluster, the statuses of zones where the cluster resides, and the basic information of nodes in the cluster.
-   Allows you to perform a switchover for faulty nodes, migrate nodes, or restart a cluster or nodes with one click. |
|Authorization management|Allows you to grant permissions to users by using the Resource Access Management \(RAM\) service or role-based access control \(RBAC\) policies.|
|Monitoring and alerting|Allows you to use the Cloud Monitor or Advanced Monitoring and Alerting service with one click to monitor the metrics of clusters and nodes and report alerts.|
|Log viewing|Allows you to view multiple types of logs, such as cluster logs, slow logs, and access logs.|
|EYou|Diagnoses clusters from multiple dimensions, analyzes possible risks, and provides the optimal solutions.|
|Security and high availability|Disaster recovery|Enables you to quickly deploy a cluster across zones, which improves the stability of upper-layer services.|
|Network configuration|Allows you to configure public and private IP address whitelists based on your business requirements. Cluster access over virtual private clouds \(VPCs\) is enabled by default.|
|Security configuration|-   Allows you to enable HTTPS with one click.
-   Allows you to use Key Management Service \(KMS\) to encrypt data at rest before you store the data in Elasticsearch. |
|Data backup|-   Enables the system to automatically back up data on a regular basis.
-   Allows you to manually back up and restore data across clusters. |
|Configuration and plug-in center|Scenario-based configuration|Provides scenario-based configuration templates to optimize default cluster configurations.|
|Cluster configuration center|Allows you to customize configurations, such as the YML file, synonym dictionary, and garbage collector \(GC\).|
|Plug-in center|Provides a variety of self-developed plug-ins by default. You can use the plug-ins to perform advanced searches, improve cluster stability, or optimize cluster performance based on your business requirements.|
|Plug-in customization|Allows you to upload custom plug-ins and dictionaries based on your business requirements.|
|Integration with other services in the Elastic Stack ecosystem|Fully managed Elastic Stack ecosystem|Provides the Logstash and Beats services to help you collect and process data.|
|Visualization management center|Provides the Kibana, Grafana, and DataV services for you to manage visualization.|
|Data migration and synchronization|Allows you to migrate or synchronize data from various self-managed Elasticsearch clusters, databases, and other big data services.|

## Capabilities and advanced commercial features provided by the open source Elastic Stack ecosystem

Alibaba Cloud Elasticsearch clusters of the Standard Edition support the following open source Elasticsearch features. For more information about the features, see [Elastic Stack features](https://www.elastic.co/cn/elastic-stack/features#-------). Due to the rapid iteration of open source Elasticsearch versions, the features supported by each version are constantly updated. For more information about the features supported by each version, see [Version comparisons]().

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

## Create an Elasticsearch cluster

-   [Create a cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create a cluster.md)
-   [Buy page parameters]()

## Get started with the Standard Edition

[Quick start](/intl.en-US/Elasticsearch Instances Management/Overview.md)

