---
keyword: [query Elasticsearch logs, Cluster Log, Search Slow Log, GC Log, Access Log, Asynchronous Write Log]
---

# Query logs

Alibaba Cloud Elasticsearch provides the logs of your Elasticsearch cluster. You can view the logs on the Logs page of the Elasticsearch console. This page contains the following tabs: Cluster Log, Search Slow Log, Indexing Slow Log, GC Log, Access Log, and Asynchronous Write Log. You can enter a keyword and specify a time range to search for specific log entries. This topic describes how to query logs and configure slow logs.

You can query log entries from a maximum of seven consecutive days. By default, the log entries are displayed by time in descending order. The Lucene query syntax is supported. For more information, see [Query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/query-dsl-query-string-query.html#query-string-syntax).

**Note:**

-   Alibaba Cloud Elasticsearch can return a maximum of 10,000 log entries for each query. If the returned log entries do not contain the expected log data, you can specify a time range when you query the log data.
-   The **Access Log** tab is displayed only for Elasticsearch V6.7.0 clusters of the Standard Edition that use the latest kernel. For more information how to update a kernel, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md).

## Procedure

The following example demonstrates how to search for the log entries of an Elasticsearch cluster. The log entries meet the following conditions: the `content` field contains the `health` keyword, the `level` field is `info`, and the `host` field is `172.16.xx.xx`.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane of the page that appears, click **Logs**.

5.  On the **Cluster Log** tab, enter a query string in the search bar.

    ![Query string](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7667819951/p61417.png)

    In this example, the query string is `host:172.16.xx.xx AND content:health AND level:info`.

    **Note:** `AND` in the query string must be uppercase.

6.  Select a start time and an end time, and click **Search**.

    **Note:**

    -   If you do not select an end time, the current system time is specified as the end time.
    -   If you do not select a start time, the default start time is one hour earlier than the end time.
    After you click Search, Elasticsearch returns the log entries that match your query string and displays them on the **Logs** page. The returned log data contains the following information: **Time**, **Node IP**, and **Content**.

    ![Log query results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7667819951/p61434.png)

    -   **Time**: the time when the log entry was generated.
    -   **Node IP**: the IP address of the node in your Elasticsearch cluster.
    -   **Content**: consists of level, host, time, and content.

        |Field|Description|
        |-----|-----------|
        |level|The level of the log entry. Log entry levels include trace, debug, info, warn, and error. A GC log entry does not contain the `level` field.|
        |host|The IP address of the node that generates the log entry.|
        |time|The time when the log entry was generated.|
        |content|The content of the log entry.|


## Configure slow logs

Elasticsearch logs only read and write operations that require 5s to 10s to complete as slow logs. This mechanism does not help identify issues such as unbalanced loads, read and write exceptions, or slow data processing. After you create an Elasticsearch cluster, log on to the Kibana console of the cluster and run the following command to reduce the timestamp precision of log entries. Then, you can capture more logs.

**Note:** For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

```
PUT _settings
{
        "index.indexing.slowlog.threshold.index.debug" : "10ms",
        "index.indexing.slowlog.threshold.index.info" : "50ms",
        "index.indexing.slowlog.threshold.index.warn" : "100ms",
        "index.search.slowlog.threshold.fetch.debug" : "100ms",
        "index.search.slowlog.threshold.fetch.info" : "200ms",
        "index.search.slowlog.threshold.fetch.warn" : "500ms",
        "index.search.slowlog.threshold.query.debug" : "100ms",
        "index.search.slowlog.threshold.query.info" : "200ms",
        "index.search.slowlog.threshold.query.warn" : "1s"
}
```

After the configuration is completed, if the time to run a read or write task is exceeded, you can query the related logs on the **Logs** page of your cluster.

## References

[ListSearchLog](/intl.en-US/API Reference/Elasticsearch instances/Query logs/ListSearchLog.md)

