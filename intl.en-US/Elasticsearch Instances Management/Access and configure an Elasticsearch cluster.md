---
keyword: [access an Elasticsearch cluster, configure an Elasticsearch cluster]
---

# Access and configure an Elasticsearch cluster

Before you use an Alibaba Cloud Elasticsearch cluster to serve your business, you must access the cluster. You can also configure the cluster to improve query efficiency and service security. This topic describes how to access an Elasticsearch cluster and provides the configurations that are supported by Alibaba Cloud Elasticsearch.

## Access an Elasticsearch cluster

You can use one of the following methods to access an Elasticsearch cluster.

|Access method|Use scenario|References|
|-------------|------------|----------|
|Kibana console|-   Analyze and present data in graphs.
-   Monitor an Elasticsearch cluster.
-   Manage data.

|[Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md)|
|APIs|Use APIs to access an Elasticsearch cluster.|[Use API operations to manage an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Developer Guide/Use API operations to manage an Alibaba Cloud Elasticsearch cluster.md)|
|Clients|Run the PHP, Python, Java, or Go code to access an Elasticsearch cluster.|[Use a client to access an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Developer Guide/Use a client to access an Alibaba Cloud Elasticsearch cluster.md)|

## Configure an Elasticsearch cluster

Alibaba Cloud Elasticsearch supports the following configurations.

|Item|Detailed configuration|References|
|----|----------------------|----------|
|Cluster configuration|-   Configure synonyms.
-   Configure a garbage collector.
-   Configure a YML file.
-   Configure a scenario-based template.

|-   [Upload a synonym dictionary file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure synonyms/Upload a synonym dictionary file.md)
-   [Configure a garbage collector](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure a garbage collector.md)
-   [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md)
-   [Use a scenario-based template to modify the configurations of a cluster](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Perform scenario-based configuration/Use a scenario-based template to modify the configurations of a cluster.md) |
|Plug-in configuration|-   Install or remove a built-in plug-in.
-   Perform a standard or rolling update for the IK analysis plug-in.
-   Upload a custom plug-in.

|-   [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md)
-   [Use the analysis-ik plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-ik plug-in.md)
-   [Upload and install a custom plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Upload and install a custom plug-in.md) |
|Data backup configuration|-   Create automatic snapshots and restore data from the automatic snapshots.
-   Create manual snapshots and restore data from the manual snapshots.
-   Configure a shared Object Storage Service \(OSS\) repository.

|-   [Create automatic snapshots and restore data from automatic snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create automatic snapshots and restore data from automatic snapshots.md)
-   [Create manual snapshots and restore data from manual snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create manual snapshots and restore data from manual snapshots.md)
-   [Configure a shared OSS repository](/intl.en-US/Elasticsearch Instances Management/Data backup/Configure a shared OSS repository.md) |
|Security settings|-   Configure the password that is used to access the Elasticsearch cluster.
-   Configure a private IP address whitelist for the Elasticsearch cluster.
-   Enable the Public Network Access feature.
-   Configure a public IP address whitelist for the Elasticsearch cluster.
-   Enable HTTPS.
-   Connect Elasticsearch clusters.

|-   [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md)
-   [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md)
-   [Enable HTTPS](/intl.en-US/Elasticsearch Instances Management/Security/Enable HTTPS.md)
-   [Connect Elasticsearch clusters](/intl.en-US/Elasticsearch Instances Management/Security/Connect Elasticsearch clusters.md) |
|Monitoring and alerting settings|-   Configure monitoring and alerting in Cloud Monitor.
-   Configure X-Pack Watcher.
-   Configure monitoring indexes.
-   Configure advanced monitoring and alerting.

|-   [Configure the monitoring and alerting feature in Cloud Monitor](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Configure the monitoring and alerting feature in Cloud Monitor.md)
-   [Configure X-Pack Watcher](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Configure X-Pack Watcher.md)
-   [Configure monitoring indexes](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Configure monitoring indexes.md) |

