---
keyword: [Logstash log query, Logstash cluster log, Logstash slow query log, Logstash GC log]
---

# Query log data

Alibaba Cloud Logstash allows you to query and view cluster, slow query, and garbage collection \(GC\) logs. You can search for specific log entries by entering keywords and setting a time range for filtering.

You can only query the logs that are generated in the last seven days. By default, the logs are displayed in descending order of time.

## Procedure

The following example searches for a cluster log entry of Logstash, which meets the following conditions: `content` contains the `running` keyword, `level` is `info`, and `host` is `172.16.xx.xx`.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Logs**.

    The **Cluster Log** tab appears.

5.  Enter the filter condition in the search bar.

    ![Log query example](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3158986061/p61089.png)

    For this example, enter `host:172.16.xx.xx AND level:info AND content:running`.

    **Note:** `AND` in the query string must be uppercase.

6.  Select a start time and an end time, and click **Search**.

    **Note:**

    -   If you do not select an end time, the current system time is specified as the end time.
    -   If you do not select a start time, the start time is the time one hour earlier than the end time.
    After the search is successful, Logstash returns the query result based on your filter condition. The returned result contains the following information: **Time**, **Node IP**, and **Content**.

    ![Logstash log query result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3158986061/p61116.png)

    -   **Time**: the time when the log entry was generated.
    -   **Node IP**: the IP address of the Logstash node.
    -   **Content**: consists of `level`, `host`, `time`, and `content`.

        |Field|Description|
        |-----|-----------|
        |`level`|The level of the log entry. Log levels include trace, debug, info, warn, and error. GC log entries do not have the `level` field.|
        |`host`|The IP address of the Logstash node. You can obtain this IP address on the Basic Information page of the Logstash cluster.|
        |`time`|The time when the log entry was generated.|
        |`content`|The content of the log entry.|


