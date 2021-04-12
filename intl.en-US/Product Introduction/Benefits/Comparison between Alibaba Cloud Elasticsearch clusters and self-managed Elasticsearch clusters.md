# Comparison between Alibaba Cloud Elasticsearch clusters and self-managed Elasticsearch clusters

Alibaba Cloud Elasticsearch provides a fully managed Elasticsearch service and is fully compatible with open source Elasticsearch. Alibaba Cloud Elasticsearch optimizes kernel performance and provides the commercial plug-in X-Pack free of charge. Alibaba Cloud Elasticsearch is out-of-the-box. It features high availability and auto scaling, and supports the pay-as-you-go billing method. This topic describes the comparisons between Alibaba Cloud Elasticsearch clusters and self-managed Elasticsearch clusters in terms of costs, cluster management, supported capabilities, security, and availability.

|Item|Alibaba Cloud Elasticsearch cluster|Self-managed Elasticsearch cluster hosted on an ECS instance|
|----|-----------------------------------|------------------------------------------------------------|
|Resource costs|-   Auto scaling is supported, which allows you to change the specifications, number, disk type, and disk space of nodes.

|-   Resources may be insufficient during peak hours and may be wasted during off-peak hours. |
|Network fees|-   You can access a cluster over an internal network free of charge by using an Elastic Compute Service \(ECS\) instance that resides in the same region as the cluster.
-   You can access a cluster over the Internet free of charge by using an ECS instance that resides in a different region from the cluster. Alibaba Cloud Elasticsearch provides the Public Network Access feature, which is enabled by default. The maximum network bandwidth for access to a cluster over the Internet is 4 GB/s.

|-   You can access a cluster over an internal network free of charge by using an ECS instance that resides in the same region as the cluster.
-   You can access a cluster over the Internet by using an ECS instance that resides in a different region from the cluster. However, you are charged for the access. For more information about the billing standards of the access, see [Public bandwidth](/intl.en-US/Pricing/Billing items/Public bandwidth.md). |
|Labor and time costs|-   Alibaba Cloud Elasticsearch, Logstash, and Kibana \(ELK\) are fully managed services. They are out-of-the-box and support the pay-as-you-go billing method.
-   A visual interface is provided for cluster O&M. This reduces O&M costs.

|-   You must purchase a machine and manually deploy a cluster. This process requires a long period of time, and the iteration of the cluster is slow.
-   A professional Elasticsearch engineer team is required to manage resources and perform cluster O&M. This results in high labor costs. |
|Costs for risk reduction|Alibaba Cloud Elasticsearch guarantees 99.9% reliability and features few IT risks, and upper-level business risks are controllable.|Service reliability is not guaranteed. In this case, technical expertise and a large number of investments are required to reduce business risks.|
|Feature costs|-   All the advanced features of the commercial plug-in X-Pack are provided free of charge.
-   The OSS-based data backup feature is provided free of charge.

|-   You must pay USD 5,000 for the X-Pack plug-in.
-   You must manually back up data, and you are charged for the storage used by backups. |

|Item|Alibaba Cloud Elasticsearch cluster|Self-managed Elasticsearch cluster hosted on an ECS instance|
|----|-----------------------------------|------------------------------------------------------------|
|Usability|-   Clusters are out-of-the-box and support auto scaling. You can modify cluster configurations based on your business requirements with one click.
-   You can [upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md) with one click.
-   The intelligent O&M system [EYou](/intl.en-US/Elasticsearch Instances Management/Operation and Maintenance/Intelligent operations and maintenance/Overview.md) is provided. It can detect the health of more than 20 items, such as clusters, nodes, and indexes. It can also diagnose and analyze exceptions.

|-   Cluster deployment is complex, and resources must be manually adjusted.
-   Data must be migrated before you upgrade the version of a cluster.
-   Cluster O&M is complex. You must run commands to view the health statuses of clusters, nodes, and indexes. |
|Capabilities for scenario support|-   All the advanced features of the X-Pack plug-in are provided free of charge.
-   [Scenario-based configuration templates](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Perform scenario-based configuration/Use a scenario-based template to modify the configurations of a cluster.md) are used to provide appropriate parameter configurations.
-   A natural language processing \(NLP\) plug-in developed by Alibaba DAMO Academy, a [vector search]() plug-in, and a self-developed SQL plug-in are provided. These plug-ins improve the performance of clusters in search scenarios.
-   Cold and hot data separation is supported, and an index compression plug-in is provided. This improves the performance of clusters in logging scenarios.

|You must develop the capabilities or integrate the capabilities of open source Elasticsearch on your own.|
|Performance|-   An enhanced [kernel](/intl.en-US/AliES Kernel/AliES release notes.md) is provided to improve read and write performance.

|You must ensure that the cluster performance meets your requirements, which is a complex process.|
|Availability|-   [Data can be automatically backed up](/intl.en-US/Elasticsearch Instances Management/Data backup/Create automatic snapshots and restore data from automatic snapshots.md).
-   Data and service reliability reaches 99.9%.
-   A self-developed [throttling](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the aliyun-qos plug-in.md) plug-in and the [slow query isolation](/intl.en-US/AliES Kernel/Use the slow query isolation feature.md) feature are provided to ensure cluster stability.
-   [Multi-zone cluster deployment](/intl.en-US/Elasticsearch Instances Management/Deploy and use a multi-zone Elasticsearch cluster.md) is supported, and an active zone-redundancy architecture is provided.

|-   You must ensure cluster availability on your own and manually back up data.
-   Disaster recovery is difficult to implement. |
|Security|-   Clusters are accessed over virtual private clouds \(VPCs\) by default.
-   X-Pack security components are provided free of charge.
-   Field-level access control is supported.
-   [Data can be transmitted after HTTPS-based encryption](/intl.en-US/Elasticsearch Instances Management/Security/Enable HTTPS.md) and can be stored after encryption.

|-   The security of ECS instances is ensured, but clusters have security risks.
-   X-Pack security components must be separately purchased. |

