---
keyword: [aliyun-qos插件, es集群限流]
---

# 使用集群限流插件（aliyun-qos）

集群限流插件（简称aliyun-qos插件）是阿里云Elasticsearch团队自研的插件，目的是为了提高集群的稳定性。该插件能够实现节点级别的读写限流，在关键时刻对指定索引降级，将流量控制在合适范围内。例如当上游业务无法进行流量控制时，尤其对于读请求业务，可根据aliyun-qos插件设置的规则，按照业务的优先级进行适当的降级，来保护Elasticsearch服务的稳定性。

## 注意事项

aliyun-qos插件为预装插件，限流功能默认关闭，不支持卸载。该插件是为了保护集群，提高稳定性，并不会对读写流量进行精准的计量。

![aliyun-qos插件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6846359951/p89247.png)

**说明：** 使用aliyun-qos插件前，您可以在插件配置页面查看是否已安装该插件。如果未安装，可参见[安装或卸载系统默认插件](/cn.zh-CN/Elasticsearch/插件配置/安装或卸载系统默认插件.md)进行安装。插件安装成功后不可卸载。

## 评估阈值

为了不影响读写请求的执行效率，aliyun-qos插件只会在单节点上进行限流计算，并不会对集群中所有节点的读写流量进行严格精准的计量，可能会导致实际的流量有所偏差。因此在使用aliyun-qos插件前，请先参考如下规则对限流阈值进行评估：

-   查询请求

    查询请求的限流阈值 = 客户端查询请求到达Elasticsearch的端到端QPS（Query Per Second）/协调节点数量

    **说明：**

    -   端到端QPS仅指查询请求到达协调节点的每秒请求数。如果没有独立的协调节点，那么协调节点数量等于数据节点数量。
    -   假设集群中有5个协调节点，客户端的查询请求流量为1000，那么查询请求的限流阈值就设置为200。由于aliyun-qos插件不会在节点间同步精确的查询流量，所以200只是最理想的情况，实际会因为节点间的请求流量不是绝对平均略有偏差，需要适当调整。
-   写入请求

    写入请求的限流阈值的计算规则与查询请求类似，但还需要根据副本数进行调整。

    例如集群中有2个数据节点、1个索引，该索引有1个shard、1个副本，每次写入10MB大小的数据。因为存在副本，所以每个数据节点上都会被写入10MB大小的数据。另外X-Pack自身的Monitor、Audit、Watcher等任务同样会占用写入流量，设置阈值时需要预留出该部分的大小。


## 开启限流功能

aliyun-qos插件的限流功能默认关闭，使用时需要先开启该功能。

**说明：** 本文中的命令均可在Kibana控制台上执行，详情请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

```
PUT _cluster/settings
{
   "transient" : {
      "apack.qos.ratelimit.enabled":"true"
   }
}
```

## 设置查询QPS

通过定义`index_patterns`设置目标索引的查询QPS（Query Per Second），支持索引和索引通配符。

-   设置单个索引的查询QPS

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "twitter",
       "search.max_queries_per_sec" : 1000
    }
    ```

-   设置指定名称前缀的索引的查询QPS

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "nginx-log-*",
       "search.max_queries_per_sec" : 1000
    }
    ```

-   设置所有索引的查询QPS

    ```
    PUT _qos/_ratelimit/<limitName>
    {
       "search.index_patterns" : "*",
       "search.max_queries_per_sec" : 2000
    }
    ```


**说明：** 您可以定义多条不同的规则，只要请求命中任意一条规则，就会触发限流。

当您在客户端或Kibana控制台上执行数据查询操作时，如果查询QPS超过`search.max_queries_per_sec`设置的值，系统会显示如下报错信息。请根据报错信息适当减少查询QPS。

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

## 设置Bulk每秒写入大小

通过设置[Bulk](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docs-bulk.html)每秒写入的总字节数，限制协调节点每秒接收的写入字节数。

```
PUT _qos/_ratelimit/<limitName>
{
   "bulk.index_patterns": "nginx-log-*",
   "bulk.max_throughput_in_bytes" : 1000000
}
```

**说明：** 您可以定义多条不同的规则，只要请求命中任意一条规则，就会触发限流。

当您在客户端或Kibana控制台上执行数据写入操作时，如果每秒写入的字节数超过`bulk.max_throughput_in_bytes`设置的值，系统会显示如下报错信息。请根据报错信息适当减少每秒写入的字节数。

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

## 设置Bulk单次请求大小

通过设置[Bulk](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docs-bulk.html)单次请求的最大值，限制协调节点接收单次请求的写入字节数。

```
PUT _qos/_ratelimit/<limitName>
{
   "bulk.index_patterns": "nginx-log-*",
   "bulk.max_request_size_in_bytes" : 1000
}
```

**说明：** 您可以定义多条不同的规则，只要请求命中任意一条规则，就会触发限流。

## 获取限流配置

-   获取所有限流配置

    ```
    GET _qos/_ratelimit
    ```

-   获取单个指定的限流配置

    ```
    GET _qos/_ratelimit/<limitName>
    ```

-   获取多个指定的限流配置

    ```
    GET _qos/_ratelimit/<limitName1,limitName2>
    ```


## 删除限流配置

```
DELETE _qos/_ratelimit/<limitName>
```

## 关闭限流功能

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

