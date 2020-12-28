---
keyword: [query Logstash logs, Logstash cluster log, Logstash slow log, Logstash GC log, Logstash debug log]
---

# Query logs

Alibaba Cloud Logstash allows you to query and view Logstash logs. The logs include cluster logs, slow logs, garbage collection \(GC\) logs, and debug logs. You can enter a keyword and specify a time range to search for specific log entries. You can query only the logs that are generated in a maximum of seven consecutive days. By default, the logs are displayed in descending order of time. This topic describes how to query the logs of a Logstash cluster.

## Procedure

In this example, a cluster log entry of Logstash is searched. The log entry meets the following conditions: `content` contains the `running` keyword, `level` is `info`, and `host` is `172.16.xx.xx`.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Logs**.

    The **Cluster Log** tab appears.

5.  Enter the query string in the search bar.

    ![Log query example](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3158986061/p61089.png)

    In this example, the query string is `host:172.16.xx.xx AND level:info AND content:running`.

    **Note:** `AND` in the query string must be uppercase.

6.  Specify a start time and an end time, and click **Search**.

    **Note:**

    -   If you do not specify an end time, the current system time is used as the end time.
    -   If you do not specify a start time, the start time is one hour earlier than the end time.
    After the search is successful, Logstash returns the query result based on your query string. The returned result contains the following information: **Time**, **Node IP**, and **Content**.

    ![Logstash log query result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3158986061/p61116.png)

    -   **Time**: the time when the log entry was generated.
    -   **Node IP**: the IP address of the Logstash node.
    -   **Content**: contains the `level`, `host`, `time`, and `content` fields.

        |Field|Description|
        |-----|-----------|
        |`level`|The level of the log entry. Log levels include trace, debug, info, warn, and error. GC log entries do not contain the `level` field.|
        |`host`|The IP address of the Logstash node. You can obtain the IP address on the Basic Information page of the Logstash cluster.|
        |`time`|The time when the log entry was generated.|
        |`content`|The content of the log entry.|


