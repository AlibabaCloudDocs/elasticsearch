# What is Alibaba Cloud Elasticsearch?

Elasticsearch is an open source, distributed, real-time search and analytics engine built on Apache Lucene. It is released under the Apache License and is a popular search engine for enterprises. It provides services based on RESTful APIs and allows you to store, query, and analyze large amounts of datasets in near real time. Elasticsearch is typically used to support complex queries and high-performance applications.

Alibaba Cloud Elasticsearch is a fully managed cloud service that is developed based on [open source Elasticsearch](https://www.elastic.co/cn/elasticsearch/features). This service is fully compatible with the features provided by open source Elasticsearch. It is out-of-the-box and supports the pay-as-you-go billing method. In addition to Elastic Stack components such as Elasticsearch, Logstash, Kibana, and Beats, Alibaba Cloud provides the X-Pack plug-in free of charge together with Elastic. X-Pack is integrated into Kibana to provide features, such as security, alerting, monitoring, and machine learning. It also provides SQL capabilities. Alibaba Cloud Elasticsearch is widely used in scenarios such as real-time log analysis and processing, information retrieval, multidimensional data queries, and statistical data analytics.

## Overview

Alibaba Cloud Elasticsearch is committed to providing a low-cost, scenario-based Elasticsearch service on the cloud based on the open source Elastic Stack ecosystem. Alibaba Cloud Elasticsearch originates from but is not limited to this ecosystem. Alibaba Cloud has superior computing and storage capabilities on the cloud and technical expertise in the fields of cluster security and O&M. This enables Alibaba Cloud Elasticsearch to support one-click deployment, auto scaling, intelligent O&M, and various kernel optimization features. Alibaba Cloud Elasticsearch also provides a complete set of solutions such as migration, disaster recovery, backup, and monitoring.

Alibaba Cloud Elasticsearch features high security, performance, and availability and provides powerful search and analytics capabilities. It simplifies cluster deployment and management, reduces resource and O&M costs, ensures data security and reliability, enables upstream and downstream data links, and optimizes read and write performance. Based on these features and optimizations, Alibaba Cloud Elasticsearch allows you to build business applications with ease, such as applications that perform log analysis, exception monitoring, enterprise search, and big data analytics. Alibaba Cloud Elasticsearch enables you to focus on the business applications themselves and add value to your business. 

## Components

The Alibaba Cloud Elastic Stack ecosystem contains the following components: Elasticsearch, Kibana, Beats, and Logstash. Elasticsearch is a real-time, distributed search and analytics engine. Kibana provides a visual interface for data analytics. Beats collects data from various machines and systems. Logstash collects, converts, processes, and generates data. Integrated with Kibana, Beats, and Logstash, Alibaba Cloud Elasticsearch can be used for real-time log processing, full-text searches, and data analytics.

-   [X-Pack](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/setup-xpack.html)

    X-Pack is a commercial extension of Elasticsearch. It provides security, alerting, monitoring, graphing, reporting, and machine learning capabilities. When you create an Alibaba Cloud Elasticsearch cluster, the system integrates X-Pack into Kibana to provide free services. The services include authorization and authentication, role-based access control, real-time monitoring, visual reporting, and machine learning. X-Pack facilitates cluster O&M and application development.

-   [Beats](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md)

    Beats is a lightweight data collection tool that integrates a variety of single-purpose data shippers. These data shippers collect data from various machines or systems and send the collected data to Logstash or Elasticsearch.

    Beats allows you to create the following types of data shippers: Filebeat, Metricbeat, Auditbeat, and Heartbeat. You can create and configure a shipper to collect various types of data from Elastic Compute Service \(ECS\) instances or Container Service for Kubernetes \(ACK\) clusters. The data include logs, network data, and container metrics. Beats also allows you to manage your shippers in a centralized manner.

-   [Logstash](/intl.en-US/Logstash/What is Alibaba Cloud Logstash?.md)

    Logstash is a server-side data processing pipeline. It uses input, filter, and output plug-ins to dynamically collect data from a variety of sources, process and convert the data, and then save it to a specific location.

    Alibaba Cloud Logstash is a fully managed service and is fully compatible with open source Logstash. Logstash allows you to quickly deploy pipelines, configure them by using a visual interface, and centrally manage them. It provides multiple types of plug-ins to connect to cloud services, such as Object Storage Service \(OSS\) and MaxCompute.

-   [Kibana](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md)

    Kibana is a flexible data analytics and visualization tool. Multiple users can log on to the Kibana console at the same time. You can use Kibana to search for, view, and manage data in Elasticsearch indexes. When you create an Alibaba Cloud Elasticsearch cluster, the system automatically deploys an independent Kibana node. This node allows you to present diversified data analytics reports and dashboards by using graphs, tables, or maps based on your business requirements.


## Related items

-   [AliES and its provided plug-ins](/intl.en-US/AliES Kernel/AliES release notes.md)

    In addition to all the features provided by the open source Elasticsearch kernel, Alibaba Cloud Elasticsearch develops the AliES kernel. This kernel enables Alibaba Cloud Elasticsearch to provide optimizations in multiple aspects, such as thread pools, monitoring metric types, circuit breaking policies, and query and write performance. The kernel also provides a variety of self-developed plug-ins to improve cluster stability, enhance performance, reduce costs, and optimize monitoring and O&M.

-   [EYou](/intl.en-US/Elasticsearch Instances Management/Operation and Maintenance/Intelligent operations and maintenance/Overview.md)

    EYou is an intelligent O&M system provided by Alibaba Cloud Elasticsearch. This system can detect the health of more than 20 items, such as clusters, nodes, and indexes. EYou simplifies cluster O&M. It comprehensively observes and records the running statuses of clusters and automatically summarizes cluster diagnostic results. It also detects the possible risks of your clusters. If your clusters are abnormal, the system quickly provides key information and reasonable optimization suggestions.


## References

-   Benefits
    -   [Comparison between Alibaba Cloud Elasticsearch clusters and self-managed Elasticsearch clusters](/intl.en-US/Product Introduction/Benefits/Comparison between Alibaba Cloud Elasticsearch clusters and self-managed Elasticsearch clusters.md)
    -   [High availability](/intl.en-US/Product Introduction/Benefits/High availability.md)
    -   [High security](/intl.en-US/Product Introduction/Benefits/High security.md)
    -   [High performance](/intl.en-US/Product Introduction/Benefits/High performance.md)
-   Cluster purchase
    -   [Purchase an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md)
    -   [Purchase an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md)
-   Quick start
    -   [Quick start of Elasticsearch](/intl.en-US/Elasticsearch Instances Management/Quick start.md)
    -   [Quick start of Logstash](/intl.en-US/Logstash/Quick Start/Overview.md)
    -   [Quick start of Beats](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md)

