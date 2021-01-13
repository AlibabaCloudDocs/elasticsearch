---
keyword: Alibaba Cloud Elasticsearch built-in plug-in
---

# Install and remove a built-in plug-in

After you purchase an Alibaba Cloud Elasticsearch cluster, the built-in plug-ins of the cluster are displayed on the Built-in Plug-ins tab of the Elasticsearch console. You can install or remove these plug-ins as required. This topic describes how to install and remove a built-in plug-in for Alibaba Cloud Elasticsearch.

**analysis-ik** and **elasticsearch-repository-oss** are both built-in plug-ins of Alibaba Cloud Elasticsearch, but they cannot be removed.

-   **analysis-ik**: an IK analysis plug-in. In addition to its open source functionalities, this plug-in supports the dynamic loading of dictionaries stored on Object Storage Service \(OSS\). You can use the [standard update or rolling update](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-ik plug-in.md) method to update dictionaries.
-   **elasticsearch-repository-oss**: In addition to its open source functionalities, this plug-in provides OSS storage while you create and restore index snapshots.

## Precautions

The installation or removal of a built-in plug-in restarts the cluster. Alibaba Cloud Elasticsearch removes only the plug-in that you select. You must confirm the operation before you can proceed.

## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane, click **Plug-ins**.

5.  On the **Built-in Plug-ins** tab, find your desired plug-in and click **Install** or **Remove** in the **Actions** column.

6.  Read the message that appears and click **OK**.

    The system then restarts the cluster. While the cluster restarts, you can view its task progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box.


## Additional information

The following table describes the built-in plug-ins that Alibaba Cloud Elasticsearch supports.

|Plug-in|Default status|Description|Supported operation|
|-------|--------------|-----------|-------------------|
|aliyun-knn|Not Installed|The vector search engine for Alibaba Cloud Elasticsearch. This plug-in uses the vector databases of Proxima, a vector search engine designed by Alibaba DAMO Academy. This plug-in allows you to use vector spaces in different search scenarios, such as image searching, video fingerprinting, facial and speech recognition, and commodity recommendation based on your preferences.|Install and Remove|
|aliyun-qos|Installed|The throttling plug-in for Alibaba Cloud Elasticsearch. This plug-in implements node-level read and write throttling and limits QPS and the size of each bulk request.|Install|
|aliyun-sql|Installed|The SQL plug-in independently developed by Alibaba Cloud Elasticsearch. This plug-in provides more SQL query capabilities.|Install and Remove|
|analysis-aliws|Not Installed|The aliws analysis plug-in for Elasticsearch.|Install, Remove, and Dictionary Configuration|
|analysis-icu|Installed|The ICU analysis plug-in for Elasticsearch. This plug-in integrates the Lucene ICU module into Elasticsearch and adds ICU analysis components.|Install and Remove|
|analysis-ik|Installed|The IK analysis plug-in for Elasticsearch. This plug-in cannot be removed.|Standard Update and Rolling Update|
|analysis-kuromoji|Installed|The Japanese \(Kuromoji\) analysis plug-in for Elasticsearch. This plug-in integrates the Lucene Kuromoji analysis module into Elasticsearch.|Install and Remove|
|analysis-phonetic|Installed|The phonetic analysis plug-in for Elasticsearch. This plug-in integrates the phonetic token filter into Elasticsearch.|Install and Remove|
|analysis-pinyin|Installed|The Pinyin analysis plug-in for Elasticsearch.|Install and Remove|
|analysis-smartcn|Installed|The smart Chinese analysis plug-in for Elasticsearch. This plug-in integrates the Lucene smart Chinese analysis module into Elasticsearch.|Install and Remove|
|apack|Installed|The plug-in that provides the physical replication and vector search features. The physical replication feature improves the write performance of clusters. The vector search feature achieves image searches.|Install|
|codec-compression|Installed|The index compression plug-in. This plug-in supports the brotli and zstd compression algorithms and provides a higher index compression ratio. This significantly reduces the storage costs of indexes.|Install and Remove|
|analysis-stconvert|Not Installed|The analysis plug-in that allows you to switch between simplified and traditional Chinese.|Install and Remove|
|elasticsearch-repository-oss|Installed|The plug-in that allows you to use Alibaba Cloud OSS to store Elasticsearch snapshots. This plug-in cannot be removed.|None|
|faster-bulk|Not Installed|The plug-in that aggregates bulk write requests in batches based on the specified maximum request size and aggregation interval. This plug-in improves write throughput and reduces write request rejections.|Install and Remove|
|ingest-attachment|Installed|The ingest processor for Elasticsearch. This plug-in uses Apache Tika to extract content.|Install and Remove|
|mapper-murmur3|Installed|The plug-in that computes the hash values of field values when you create an index and stores the hash values in the index.|Install and Remove|
|mapper-size|Installed|The plug-in that allows you to record the size of documents before they are compressed when you create an index.|Install and Remove|
|repository-hdfs|Installed|The plug-in that provides support for Hadoop Distributed File System \(HDFS\) repositories.|Install and Remove|

