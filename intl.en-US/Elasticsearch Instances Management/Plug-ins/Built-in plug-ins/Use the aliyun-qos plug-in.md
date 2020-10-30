---
keyword: [aliyun-qos plug-in, Elasticsearch cluster throttling]
---

# Use the aliyun-qos plug-in

aliyun-qos is a throttling plug-in developed by Alibaba Cloud Elasticsearch. This plug-in is designed to improve cluster stability. It implements node-level read/write throttling and reduces the priority of specified indexes when required. You can use the aliyun-qos plug-in to reduce the priorities of services based on the rules predefined by the plug-in. This is helpful, if you cannot implement throttling on your upstream services, especially on read requests.

## Precautions

aliyun-qos is a built-in plug-in. By default, the throttling feature is disabled, and the plug-in cannot be uninstalled. aliyun-qos is designed to improve cluster stability. It does not perform a precise measurement on read and write traffic.

![aliyun-qos plug-in](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9567819951/p89247.png)

**Note:** You can check whether you have installed the plug-in on the plug-in configuration page before you use it. If the plug-in is not installed, install it first. For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Install and remove a built-in plug-in.md). The plug-in cannot be uninstalled after it is installed.

## Evaluate thresholds

To ensure the execution efficiency of read and write requests, the aliyun-qos plug-in performs throttling only on a single node and does not precisely measure the read and write traffic of all nodes in the cluster. This may cause inconsistencies between the calculated threshold and the actual threshold. Before you use the aliyun-qos plug-in, evaluate throttling thresholds based on the following rules:

-   Query requests

    Throttling threshold for query requests = End-to-end queries per second \(QPS\) from a client to Elasticsearch/Number of client nodes

    **Note:**

    -   End-to-end QPS indicates only the number of query requests that are sent to client nodes per second. If an Elasticsearch cluster has no independent client nodes, the number of client nodes is equal to the number of data nodes.
    -   Assume that a cluster has five client nodes and the end-to-end QPS is 1,000. The throttling threshold for query requests is 200. The aliyun-qos plug-in does not precisely synchronize query traffic between nodes. Therefore, 200 is only an approximate value. In actual situations, the query traffic is not evenly distributed between nodes, and the value needs to be adjusted.
-   Write requests

    The throttling threshold for write requests is calculated by using a similar method to that for query requests, and it needs to be adjusted based on the number of replica shards.

    For example, a cluster has two data nodes and one index, the index has one primary shard and one replica shard, and 10 MiB of data is written each time. In this case, 10 MiB of data is written to each data node each time because the index has one replica shard. In addition, X-Pack Monitor, Audit, and Watcher tasks also generate write traffic. You must consider these tasks when you set the throttling threshold.


## Enable throttling

By default, the throttling feature of aliyun-qos is disabled. You must enable the feature before you use it.

**Note:** You can run the commands provided in this topic in the Kibana console. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

```
PUT _cluster/settings
{
   "transient" : {
      "apack.qos.ratelimit.enabled":"true"
   }
}
```

## Set query QPS

You can define the `index_patterns` parameter to set query QPS for a specific index or indexes specified by using a wildcard.

-   Set query QPS for a specific index

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "twitter",
       "search.max_times_sec" : 1000
    }
    ```

-   Set query QPS for indexes with a specified prefix

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "nginx-log-*",
       "search.max_times_sec" : 1000
    }
    ```

-   Set query QPS for all indexes

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "*",
       "search.max_times_sec" : 2000
    }
    ```


**Note:** You can define multiple rules to trigger throttling. If a request hits one of the rules, throttling is triggered.

If query QPS exceeds the value specified by `search.max_times_sec` when you query data on your client or in the Kibana console, the system displays the following error message. In this case, you must reduce query QPS. The following example shows the preceding error message:

```
{
  "error": {
    "root_cause": [
      {
        "type": "rate_limited_exception",
        "reason": "request indices:data/read/search rejected, limited by [l1:t*:1.0]"
      }
    ],
    "type": "rate_limited_exception",
    "reason": "request indices:data/read/search rejected, limited by [l1:t*:1.0]"
  },
  "status": 429
}
```

## Set bulk.max\_bytes\_sec

You can set bulk.max\_bytes\_sec to limit the maximum number of bytes that a client node receives per second for all bulk requests. For more information, see [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docs-bulk.html).

```
PUT _qos/_ratelimit/<limitName>
{
   "bulk.max_bytes_sec" : 1000000
}
```

**Note:** You can define multiple rules to trigger throttling. If a request hits one of the rules, throttling is triggered.

If the number of bytes to write per second exceeds the value specified by `bulk.max_bytes_sec` when you write data on your client or in the Kibana console, the system displays the following error message. In this case, you must reduce the number of bytes to write per second.

```
{
  "error": {
    "root_cause": [
      {
        "type": "rate_limited_exception",
        "reason": "request indices:data/write/bulk rejected, limited by [b2:ByteSizePreSeconds:992.0]"
      }
    ],
    "type": "rate_limited_exception",
    "reason": "request indices:data/write/bulk rejected, limited by [b2:ByteSizePreSeconds:992.0]"
  },
  "status": 413
}
```

## Set bulk.max\_bytes\_pre

You can set bulk.max\_bytes\_pre to limit the maximum number of bytes that a client node receives for a single bulk request. For more information, visit [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docs-bulk.html).

```
PUT _qos/_ratelimit/<limitName>
{
   "bulk.max_bytes_pre" : 1000
}
```

**Note:** You can define multiple rules to trigger throttling. If a request hits one of the rules, throttling is triggered.

## Obtain throttling rules

-   Obtain all throttling rules

    ```
    GET _qos/_ratelimit
    ```

-   Obtain a specific throttling rule

    ```
    GET _qos/_ratelimit/<limitName>
    ```

-   Obtain specific throttling rules

    ```
    GET _qos/_ratelimit/<limitName1,limitName2>
    ```


## Delete a throttling rule

```
DELETE _qos/_ratelimit/<limitName>
```

## Disable throttling

```
PUT _cluster/settings
{
   "transient" : {
      "apack.qos.ratelimit.enabled":"false"
   }
}
```

```
PUT _cluster/settings
{
   "transient" : {
      "apack.qos.ratelimit.enabled":null
   }
}
```

