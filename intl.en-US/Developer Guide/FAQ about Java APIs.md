# FAQ about Java APIs

This topic provides answers to some frequently asked questions about the Java APIs of Alibaba Cloud Elasticsearch.

-   [When I use Transport Client to access an Elasticsearch cluster, how do I set the cluster.name parameter?](#section_dbl_l63_3we)
-   [When I use Transport Client to access an Elasticsearch cluster, the error message "NoNodeAvailableException" is displayed. What do I do?](#section_1zl_g2x_4b0)

## When I use Transport Client to access an Elasticsearch cluster, how do I set the cluster.name parameter?

Set this parameter to the ID of your cluster. You can obtain the ID from the Basic Information page of your cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).

## When I use Transport Client to access an Elasticsearch cluster, the error message "NoNodeAvailableException" is displayed. What do I do?

The error message returned because Transport Client 5.5 or 5.6 is used. We recommend that you use Transport Client 5.3.3 to access an Elasticsearch cluster. Transport Client can be used to access only Elasticsearch V5.5 or V5.6 clusters. When you use Transport Client, set the `client.transport.sniff` parameter to false. For more information, see [Transport Client \(5.x\)](/intl.en-US/Developer Guide/Java API/Transport Client (5.x).md).

**Note:** Transport Client falls into disuse in open source Elasticsearch 7.0. We recommend that you use Java REST clients. For more information about Java REST clients, see [High Level REST Client \(6.3.x\)](/intl.en-US/Developer Guide/Java API/High Level REST Client (6.3.x).md), [High Level REST Client \(6.7.x\)](/intl.en-US/Developer Guide/Java API/High Level REST Client (6.7.x).md), and [Low Level REST Client \(5.x\)](/intl.en-US/Developer Guide/Java API/Low Level REST Client (5.x).md).

