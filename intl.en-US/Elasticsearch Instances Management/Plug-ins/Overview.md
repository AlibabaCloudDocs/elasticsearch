# Overview

Alibaba Cloud Elasticsearch provides more than 20 plug-ins, which include open source plug-ins and self-developed plug-ins. The plug-ins improve the stability, query performance, write performance, tokenized queries, and data searches of Alibaba Cloud Elasticsearch clusters. This topic describes the built-in plug-ins and custom plug-ins that are supported by Alibaba Cloud Elasticsearch.

## Built-in plug-ins

You can install or remove built-in plug-ins based on your business requirements. For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md).

The following tables describe the built-in plug-ins and provide the Elasticsearch versions in which the plug-ins are available.

-   Self-developed plug-ins

    |Plug-in|Description|Applicable Elasticsearch version|Supported operation|
    |-------|-----------|--------------------------------|-------------------|
    |[analysis-aliws](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-aliws plug-in.md)|The analysis plug-in. This plug-in integrates an analyzer and a tokenizer into Elasticsearch. This allows you to search for and analyze documents.|V6.0 and later versions|Install, Remove, and Dictionary Configuration|
    |[aliyun-sql](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the aliyun-sql plug-in/Use method.md)|The SQL plug-in that is used to parse SQL statements. This plug-in allows you to execute SQL statements to query data in Elasticsearch by using the same way you query data in common databases.|V6.7.0 and later versions|Install and Remove|
    |[apack](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md)|The plug-in that provides the physical replication and vector search features. The physical replication feature improves the write performance of clusters. The vector search feature implements image searches.|V6.7.0 \(with a kernel version of V1.2.0 or later\)|Install|
    |[aliyun-knn]()|The vector search engine. This plug-in allows you to use vector spaces in different search scenarios, such as image searching, video fingerprinting, facial and speech recognition, and commodity recommendation, based on your preferences.|V6.7.0 and later versions \(with a kernel version of V1.2.0 or later\)**Note:** If the cluster version is V6.7.0 and the kernel version is V1.2.0 or later, the aliyun-knn plug-in is integrated into the apack plug-in. The apack plug-in is installed by default. If the cluster version is later than V6.7.0, or the cluster version is V6.7.0 and the kernel version is earlier than V1.2.0, you must manually install the aliyun-knn plug-in on the Plug-ins page in the Elasticsearch console.

|Install and Remove|
    |[faster-bulk](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the faster-bulk plug-in.md)|The plug-in that aggregates bulk write requests in batches. This plug-in implements the aggregation based on the specified maximum request size and aggregation interval. This plug-in improves write throughput and reduces write request rejections.|6.7.0|Install and Remove|
    |[codec-compression](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the codec-compression plug-in of the beta version.md)|The index compression plug-in. This plug-in supports the brotli and zstd compression algorithms and provides a higher index compression ratio. This significantly reduces the storage costs of indexes.|6.7.0|Install and Remove|
    |[aliyun-qos](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the aliyun-qos plug-in.md)|The throttling plug-in. This plug-in implements node-level read and write throttling, and reduces the priority of specified indexes when required.|All versions|Install|
    |[gig](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the gig plug-in.md)|The plug-in that implements throttling for client nodes in an Elasticsearch cluster. This plug-in performs a switchover within seconds to address query jitters, and detects traffic to handle the surges in query latency caused by enabled warm nodes.|V6.7.0 \(with a kernel version of V1.3.0\)|Install and Remove|

-   Open source plug-ins

    |Plug-in|Description|Applicable Elasticsearch version|Supported operation|
    |-------|-----------|--------------------------------|-------------------|
    |[analysis-ik](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-ik plug-in.md)|The IK analysis plug-in. This plug-in integrates the [IK analyzer of Apache Lucene](http://code.google.com/p/ik-analyzer/) into Elasticsearch and supports custom dictionaries.|All versions|Standard Update and Rolling Update|
    |[analysis-icu](https://www.elastic.co/guide/en/elasticsearch/plugins/6.7/analysis-icu.html)|The ICU analysis plug-in. This plug-in integrates the Lucene ICU module and ICU analysis components into Elasticsearch.|All versions|Install and Remove|
    |[analysis-kuromoji](https://www.elastic.co/guide/en/elasticsearch/plugins/6.7/analysis-kuromoji.html)|The Japanese \(Kuromoji\) analysis plug-in. This plug-in integrates the Lucene Kuromoji analysis module into Elasticsearch.|All versions|Install and Remove|
    |[analysis-phonetic](https://www.elastic.co/guide/en/elasticsearch/plugins/6.7/analysis-phonetic.html)|The phonetic analysis plug-in. This plug-in integrates the phonetic token filter into Elasticsearch.|All versions|Install and Remove|
    |[analysis-pinyin](https://github.com/medcl/elasticsearch-analysis-pinyin)|The Pinyin analysis plug-in.|All versions|Install and Remove|
    |[analysis-smartcn](https://www.elastic.co/guide/en/elasticsearch/plugins/6.7/analysis-smartcn.html)|The smart Chinese analysis plug-in. This plug-in integrates the Lucene smart Chinese analysis module into Elasticsearch.|All versions|Install and Remove|
    |[elasticsearch-repository-oss](https://github.com/aliyun/elasticsearch-repository-oss)|The plug-in that stores snapshots. This plug-in allows you to use Alibaba Cloud Object Storage Service \(OSS\) to store Elasticsearch snapshots.|5.x|None|
    |[ingest-attachment](https://www.elastic.co/guide/en/elasticsearch/plugins/current/ingest-attachment.html#ingest-attachment)|The ingest attachment processor. This plug-in uses Apache Tika to extract content.|All versions|Install and Remove|
    |[ingest-geoip](https://www.elastic.co/guide/en/elasticsearch/plugins/current/ingest-geoip.html)|The ingest geoip processor. This plug-in queries geographic data in MaxMind databases based on IP addresses.|5.x|Install and Remove|
    |[ingest-user-agent](https://www.elastic.co/guide/en/elasticsearch/plugins/current/ingest-user-agent.html)|The ingest user\_agent processor. This plug-in extracts information from a user agent.|5.x|Install and Remove|
    |[analysis-stconvert](https://github.com/medcl/elasticsearch-analysis-stconvert)|The analysis plug-in that allows you to switch between simplified and traditional Chinese.|7.x|Install and Remove|
    |[mapper-attachments](https://github.com/elastic/elasticsearch-mapper-attachments)|The plug-in that allows you to use [Apache Tika](http://lucene.apache.org/tika/) to add attachments of more than 1,000 types of formats, such as PPT, XLS, and PDF, when you create indexes.|5.x|Install and Remove|
    |[mapper-murmur3](https://www.elastic.co/guide/en/elasticsearch/plugins/current/mapper-murmur3.html)|The plug-in that computes the hash values of field values when you create an index and stores the hash values in the index.|All versions|Install and Remove|
    |[mapper-size](https://www.elastic.co/guide/en/elasticsearch/plugins/current/mapper-size.html)|The plug-in that allows you to record the size of documents before they are compressed when you create an index.|All versions|Install and Remove|
    |[repository-hdfs](https://github.com/elastic/elasticsearch-hdfs)|The plug-in that provides support for Hadoop Distributed File System \(HDFS\) repositories.|All versions|Install and Remove|
    |[sql](https://github.com/NLPchina/elasticsearch-sql)|The SQL query plug-in.|5.5.3|Install and Remove|
    |[x-pack](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/setup-xpack.html)|An Elasticsearch extension that provides security, alerting, monitoring, reporting, and graph capabilities in an all-in-one easy-to-install package. X-Pack is integrated into Kibana to provide capabilities, such as authorization and authentication, role-based access control, real-time monitoring, visual reporting, and machine learning.|5.x|None|


## Custom plug-ins

You can upload, install, and remove custom plug-ins to meet your business requirements. For more information, see [Upload and install a custom plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Upload and install a custom plug-in.md).

