# Compatibility matrixes

This topic describes the compatibility among the versions of open source Elasticsearch, Logstash, and Beats.

## Elasticsearch compatibility \(5.x and later\)

**Note:**

-   ^: The compatibility between Elasticsearch and other services when Elasticsearch is used as an output \(index data is synchronized to Elasticsearch by using Beats or Logstash\).
-   \*: We recommend that you use the latest versions of Beats, Logstash, and ES-Hadoop. Earlier versions may not support all the desired features.
-   \*\*: In Elasticsearch 6.3 and later, all the features of X-Pack are delivered with the default distributions of Elastic Stack. For more information, see [X-Pack](https://www.elastic.co/cn/what-is/open-x-pack).

|Elasticsearch|Kibana|X-Pack|Beats^\*|Logstash^\*|ES-Hadoop \(jar\)\*|APM Server|App Search|
|-------------|------|------|--------|-----------|-------------------|----------|----------|
|5.0.x|5.0.x|5.0.x|1.3.x to 5.6.x|2.4.x to 5.6.x|5.0.x to 5.6.x|N/A|N/A|
|5.1.x|5.1.x|5.1.x|1.3.x to 5.6.x|2.4.x to 5.6.x|5.0.x to 5.6.x|N/A|N/A|
|5.2.x|5.2.x|5.2.x|1.3.x to 5.6.x|2.4.x to 5.6.x|5.0.x to 5.6.x|N/A|N/A|
|5.3.x|5.3.x|5.3.x|1.3.x to 5.6.x|2.4.x to 5.6.x|5.0.x to 5.6.x|N/A|N/A|
|5.4.x|5.4.x|5.4.x|1.3.x to 5.6.x|2.4.x to 5.6.x|5.0.x to 5.6.x|N/A|N/A|
|5.5.x|5.5.x|5.5.x|1.3.x to 5.6.x|2.4.x to 5.6.x|5.0.x to 5.6.x|N/A|N/A|
|5.6.x|5.6.x|5.6.x|1.3.x to 6.0.x|2.4.x to 6.0.x|5.0.x to 6.0.x|N/A|N/A|
|6.0.x|6.0.x|6.0.x|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|N/A|N/A|
|6.1.x|6.1.x|6.1.x|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|N/A|N/A|
|6.2.x|6.2.x|6.2.x|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|6.2.x to 6.8.x|N/A|
|6.3.x|6.3.x|N/A\*\*|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|6.2.x to 6.8.x|N/A|
|6.4.x|6.4.x|N/A\*\*|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|6.2.x to 6.8.x|N/A|
|6.5.x|6.5.x|N/A\*\*|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|6.2.x to 6.8.x|N/A|
|6.6.x|6.6.x|N/A\*\*|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|6.2.x to 6.8.x|N/A|
|6.7.x|6.7.x|N/A\*\*|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|6.2.x to 6.8.x|N/A|
|6.8.x|6.8.x|N/A\*\*|5.6.x to 6.8.x|5.6.x to 6.8.x|6.0.x to 6.8.x|6.2.x to 6.8.x|N/A|
|7.0.x|7.0.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|N/A|
|7.1.x|7.1.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|N/A|
|7.2.x|7.2.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|7.2.x|
|7.3.x|7.3.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|7.3.x|
|7.4.x|7.4.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|7.4.x|
|7.5.x|7.5.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|7.5.x|
|7.6.x|7.6.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|7.6.x|
|7.7.x|7.7.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|N/A\*\*\*\*|
|7.8.x|7.8.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|N/A\*\*\*\*|
|7.9.x|7.9.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|N/A\*\*\*\*|
|7.10.x|7.10.x|N/A\*\*|6.8.x to 7.13.x|6.8.x to 7.13.x|7.0.x to 7.13.x|7.0.x to 7.13.x\*\*\*|N/A\*\*\*\*|

We recommend that all Elasticsearch, Kibana, Filebeat, and Logstash clusters use the same minor version.

## Logstash compatibility

**Note:**

-   \*: The compatibility of monitoring and managing Elasticsearch clusters, including clusters specified by `xpack.monitoring.elasticsearch.url` and `xpack.management.elasticsearch.url`. We recommend that all Elasticsearch, Kibana, and Logstash clusters use the same minor version. This helps achieve the best performance of cluster monitoring and management. If you want to monitor and manage clusters of 6.2 or earlier, you must install X-Pack on all these services.
-   \*\*: Only Elasticsearch can be used as an output for Functionbeat in versions earlier than 7.4. Logstash and other services are not supported. Both Logstash and Elasticsearch can be used as outputs for Functionbeat in 7.4 and later.

|Logstash|Beats\*\*|Monitoring and management of Elasticsearch clusters\*|
|--------|---------|-----------------------------------------------------|
|2.4.x|1.0.x to 5.6.x|N/A|
|5.0.x|1.3.x to 5.6.x|N/A|
|5.1.x|5.0.x to 5.6.x|N/A|
|5.2.x|5.0.x to 5.6.x|5.2.x to 5.6.x|
|5.3.x|5.0.x to 5.6.x|5.3.x to 5.6.x|
|5.4.x|5.0.x to 5.6.x|5.4.x to 5.6.x|
|5.5.x|5.0.x to 5.6.x|5.5.x to 5.6.x|
|5.6.x|5.6.x to 6.8.x|5.6.x to 6.0.x|
|6.0.x|5.6.x to 6.8.x|6.0.x to 6.8.x|
|6.1.x|5.6.x to 6.8.x|6.1.x to 6.8.x|
|6.2.x|5.6.x to 6.8.x|6.2.x to 6.8.x|
|6.3.x|5.6.x to 6.8.x|6.3.x to 6.8.x|
|6.4.x|5.6.x to 6.8.x|6.4.x to 6.8.x|
|6.5.x|5.6.x to 6.8.x|6.5.x to 6.8.x|
|6.6.x|5.6.x to 6.8.x|6.6.x to 6.8.x|
|6.7.x|5.6.x to 6.8.x|6.7.x to 6.8.x|
|6.8.x|5.6.x to 6.8.x|6.8.x|
|7.0.x|6.8.x to 7.13.x|7.0.x to 7.13.x|
|7.1.x|6.8.x to 7.13.x|7.1.x to 7.13.x|
|7.2.x|6.8.x to 7.13.x|7.2.x to 7.13.x|
|7.3.x|6.8.x to 7.13.x|7.3.x to 7.13.x|
|7.4.x|6.8.x to 7.13.x|7.4.x to 7.13.x|
|7.5.x|6.8.x to 7.13.x|7.5.x to 7.13.x|
|7.6.x|6.8.x to 7.13.x|7.6.x to 7.13.x|
|7.7.x|6.8.x to 7.13.x|7.7.x to 7.13.x|
|7.8.x|6.8.x to 7.13.x|7.8.x to 7.13.x|
|7.9.x|6.8.x to 7.13.x|7.9.x to 7.13.x|
|7.10.x|6.8.x to 7.13.x|7.10.x to 7.13.x|

## Beats compatibility

**Note:**

-   \*: The compatibility of monitoring Elasticsearch clusters, including clusters specified by `xpack.monitoring.elasticsearch`. We recommend that all Elasticsearch, Kibana, and Logstash clusters use the same minor version. This helps achieve the best performance of cluster monitoring. If you want to monitor clusters of 6.2 or earlier, you must install X-Pack on all these services.
-   \*\*: Only Elasticsearch can be used as an output for Functionbeat. Logstash and other services are not supported.

|Beats\*\*|Logstash|Monitoring of Elasticsearch clusters\*|
|---------|--------|--------------------------------------|
|1.3.x|2.0.x to 5.0.x|N/A|
|5.0.x|2.0.x to 5.6.x|N/A|
|5.1.x|2.0.x to 5.6.x|N/A|
|5.2.x|2.0.x to 5.6.x|N/A|
|5.3.x|2.0.x to 5.6.x|N/A|
|5.4.x|2.0.x to 5.6.x|N/A|
|5.5.x|2.0.x to 5.6.x|N/A|
|5.6.x|5.6.x to 6.8.x|N/A|
|6.0.x|5.6.x to 6.8.x|N/A|
|6.1.x|5.6.x to 6.8.x|N/A|
|6.2.x|5.6.x to 6.8.x|6.2.x|
|6.3.x|5.6.x to 6.8.x|6.3.x to 6.8.x|
|6.4.x|5.6.x to 6.8.x|6.4.x to 6.8.x|
|6.5.x|5.6.x to 6.8.x|6.5.x to 6.8.x|
|6.6.x|5.6.x to 6.8.x|6.6.x to 6.8.x|
|6.7.x|5.6.x to 6.8.x|6.7.x to 6.8.x|
|6.8.x|5.6.x to 6.8.x|6.8.x to 7.13.x|
|7.0.x|6.8.x to 7.13.x|7.0.x to 7.13.x|
|7.1.x|6.8.x to 7.13.x|7.1.x to 7.13.x|
|7.2.x|6.8.x to 7.13.x|7.2.x to 7.13.x|
|7.3.x|6.8.x to 7.13.x|7.3.x to 7.13.x|
|7.4.x|6.8.x to 7.13.x|7.4.x to 7.13.x|
|7.5.x|6.8.x to 7.13.x|7.5.x to 7.13.x|
|7.6.x|6.8.x to 7.13.x|7.6.x to 7.13.x|
|7.7.x|6.8.x to 7.13.x|7.7.x to 7.13.x|
|7.8.x|6.8.x to 7.13.x|7.8.x to 7.13.x|
|7.9.x|6.8.x to 7.13.x|7.9.x to 7.13.x|
|7.10.x|6.8.x to 7.13.x|7.10.x to 7.13.x|

For more information about the compatibility between these services, see [Product Compatibility](https://www.elastic.co/cn/support/matrix#matrix_compatibility).

