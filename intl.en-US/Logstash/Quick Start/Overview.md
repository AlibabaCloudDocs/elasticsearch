# Overview

This topic describes how to quickly create an Alibaba Cloud Logstash cluster, configure Logstash pipelines, and synchronize data between Alibaba Cloud Elasticsearch clusters.

## Background information

Make sure that you have understood the following information:

-   [What is Alibaba Cloud Elasticsearch?](/intl.en-US/Product Introduction/What is Alibaba Cloud Elasticsearch?.md)
-   [What is Alibaba Cloud Logstash?](/intl.en-US/Logstash/What is Alibaba Cloud Logstash?.md)

## Procedure

1.  Make preparations.

    For more information, see [Preparations](/intl.en-US/Logstash/Quick Start/Preparations.md). Preparations include creating a Virtual Private Cloud \(VPC\) and a vSwitch, creating source and destination Alibaba Cloud Elasticsearch clusters, enabling the Auto Indexing feature for the destination Alibaba Cloud Elasticsearch cluster, and preparing data.

2.  [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).
3.  [Create and run a data synchronization task](/intl.en-US/Logstash/Quick Start/Step 2: Create and run a data synchronization task.md).

    Create and configure an Alibaba Cloud Logstash pipeline to synchronize data.

4.  [View data synchronization results](/intl.en-US/Logstash/Quick Start/Step 3: View data synchronization results.md).

    You can log on to the Kibana console of the destination Elasticsearch cluster to view the data synchronization results.


