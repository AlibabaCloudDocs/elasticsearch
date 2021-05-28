---
keyword: [aliyun-qos plug-in, Elasticsearch cluster throttling]
---

# Use the aliyun-qos plug-in

aliyun-qos is a throttling plug-in developed by Alibaba Cloud Elasticsearch. This plug-in is designed to improve cluster stability. The plug-in implements cluster-level read/write throttling and reduces the priorities of specified indexes based on actual situations. You can use the aliyun-qos plug-in to reduce the priorities of services based on the rules predefined by the plug-in. This is helpful, if you cannot implement throttling on your upstream services, especially on read requests.

## Prerequisites

The plug-in is upgraded to rc4 or later.

You can [log on to the Kibana console of your cluster](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) and run the `GET /_cat/plugins?v` command to check the version of the plug-in. If the version of the plug-in is earlier than rc4, you must [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm) to contact Elasticsearch technical engineers to upgrade the plug-in. After the upgrade, you must restart your Elasticsearch cluster to make the change take effect.

![Check the version of the aliyun-qos plug-in](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9195612261/p244629.png)

**Note:** If the version of the aliyun-qos plug-in is earlier than rc4, the `unsupported_operation_exception` error is reported when you use the plug-in.

## Precautions

aliyun-qos is a built-in plug-in and cannot be uninstalled. By default, the throttling feature is disabled. This plug-in is designed to protect clusters and improve cluster stability. It does not precisely measure read and write traffic.

![aliyun-qos plug-in](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9567819951/p89247.png)

**Note:** Before you use the plug-in, you can check whether you have installed the plug-in on the plug-in configuration page. If the plug-in is not installed, install it first. For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md). The plug-in cannot be uninstalled after it is installed.

## Evaluate thresholds

To ensure the processing efficiency of read and write requests, the aliyun-qos plug-in performs throttling on your cluster but does not precisely measure the read and write traffic of all the nodes in the cluster. This may cause inconsistencies between the calculated thresholds and the actual thresholds. Before you use the aliyun-qos plug-in, you can evaluate throttling thresholds based on the following rules:

-   Query requests

    Throttling threshold for query requests = End-to-end queries per second \(QPS\) from a client to Elasticsearch

    **Note:** End-to-end QPS indicates the number of query requests that are sent only to client nodes per second.

-   Write requests

    The throttling threshold for write requests is calculated by using a similar method to that for query requests but must be adjusted based on the number of replica shards.

    For example, a cluster contains two data nodes and stores one index that has one primary shard and one replica shard, and 10 MB of data is written each time. In this case, 10 MB of data is written to each data node each time because the index has one replica shard. In addition, X-Pack Monitor, Audit, and Watcher tasks also generate write traffic. You must consider these tasks when you set the throttling threshold.


## Enable throttling

By default, the throttling feature of the aliyun-qos plug-in is disabled. You must enable the feature before you use the plug-in.

**Note:** You can run all the commands provided in this topic in the Kibana console. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

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
       "search.max_queries_per_sec" : 1000
    }
    ```

-   Set query QPS for indexes with a specified prefix

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "nginx-log-*",
       "search.max_queries_per_sec" : 1000
    }
    ```

-   Set query QPS for all indexes

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "*",
       "search.max_queries_per_sec" : 2000
    }
    ```


**Note:** You can define multiple rules to trigger throttling. If a request hits one of the rules, throttling is triggered.

If the query QPS exceeds the value specified by `search.max_queries_per_sec` when you query data on your client or in the Kibana console, the system displays the following error message. If this occurs, you must reduce the query QPS.

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

## Set the maximum number of bytes that can be written per second by using bulk requests

You can set bulk.max\_throughput\_in\_bytes to limit the maximum number of bytes that a client node can receive per second for all bulk requests. For more information about bulk requests, see [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docs-bulk.html).

```
PUT _qos/_ratelimit/<limitName>
{
   "bulk.index_patterns": "nginx-log-*",
   "bulk.max_throughput_in_bytes" : 1000000
}
```

**Note:** You can define multiple rules to trigger throttling. If a request hits one of the rules, throttling is triggered.

You can perform data write operations on your client or in the Kibana console. If the number of bytes that are written per second exceeds the value of `bulk.max_throughput_in_bytes`, the system displays the following error message. If this occurs, you must reduce the number of bytes to write per second.

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

## Set the maximum number of bytes that can be written for a single bulk request

You can set bulk.max\_request\_size\_in\_bytes to limit the maximum number of bytes that a client node can receive for a single bulk request. For more information about bulk requests, see [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docs-bulk.html).

```
PUT _qos/_ratelimit/<limitName>
{
   "bulk.index_patterns": "nginx-log-*",
   "bulk.max_request_size_in_bytes" : 1000
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

