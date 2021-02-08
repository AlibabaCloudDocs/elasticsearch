# ListSearchLog

调用ListSearchLog，查看实例日志。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListSearchLog&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/search-log HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|query|String|Query|是|host:172.16.\*\*.\*\* AND content:netty|要查询的关键词。 |
|type|String|Query|是|INSTANCELOG|日志类型。可选值：

 -   **INSTANCELOG**：主日志
-   **SEARCHSLOW**：searching慢日志
-   **INDEXINGSLOW**：indexing慢日志
-   **JMVLOG**：GC日志
-   **ES\_SEARCH\_ACCESS\_LOG**：ES访问日志 |
|beginTime|Long|Query|否|1531910852074|日志开始时间戳，单位：毫秒。 |
|endTime|Long|Query|否|1531910852074|日志结束时间戳，单位：毫秒。 |
|page|Integer|Query|否|1|插件列表的页码。起始值：1，默认值：1。 |
|size|Integer|Query|否|20|分页查询时设置的每页条数。默认值：20，最小值：1，最大值：50。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|1000|实例总记录数。 |
|RequestId|String|7F40EAA1-6F1D-4DD9-8DB8-C5F00C4E\*\*\*\*|请求ID。 |
|Result|Array of Result| |请求返回的日志列表。 |
|content|String|\[GC \(Allocation Failure\) 2018-07-19T17:24:20.682+0800: 7516.513: \[ParNew: 6604768K-\>81121K\(7341504K\), 0.0760606 secs\] 7226662K-\>703015K\(31813056K\), 0.0762507 secs\] \[Times: user=0.52 sys=0.00, real=0.07 secs\]|日志详细内容。 |
|host|String|192.168.\*\*.\*\*|生成日志的节点的IP地址。 |
|instanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|level|String|info|日志等级。取值包括：

 -   warn：警告日志
-   info：信息日志
-   error：错误日志
-   trace：跟踪日志
-   debug：调试日志 |
|timestamp|Long|1531985112420|日志生成的时间戳，单位为ms。 |

Result中还包含以下参数。

|名称

|类型

|示例值

|描述 |
|----|----|-----|----|
|time

|String

|2020-07-21T11:12:53.057Z

|日志产生的时间。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/search-log?type=INSTANCELOG&query=host:172.16.**.** AND content:netty&beginTime=1531910852074&endTime=1531910852074&page=1&size=20 HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <timestamp>1552868172741</timestamp>
    <host>192.168.**.**</host>
    <contentCollection>
        <level>info</level>
        <host>192.168.**.**</host>
        <time>2019-03-18T08:16:12.741Z</time>
        <content>[o.e.c.r.a.AllocationService] [MnNASM_] Cluster health status changed from [YELLOW] to [GREEN] (reason: [shards started [[my_index][3]] ...]).</content>
    </contentCollection>
    <instanceId>es-cn-n6w1o1x0w001c****</instanceId>
</Result>
<Result>
    <timestamp>1552838205462</timestamp>
    <host>192.168.**.**</host>
    <contentCollection>
        <level>info</level>
        <host>192.168.**.**</host>
        <time>2019-03-17T23:56:45.462Z</time>
        <content>[o.e.c.r.a.AllocationService] [v4p9o7A] Cluster health status changed from [GREEN] to [YELLOW] (reason: [{MnNASM_}{MnNASM_OSR-2YgySSc****}{EvJHPrAOS_u8J3-6qZ****}{192.168.**.**}{192.168.**.**:9300}{ml.max_open_jobs=10, ml.enabled=true} transport disconnected]).</content>
    </contentCollection>
    <instanceId>es-cn-n6w1o1x0w001c****</instanceId>
</Result>
<RequestId>121753D9-744A-4203-9EC4-F29E628A****</RequestId>
<Headers>
    <X-Total-Count>2</X-Total-Count>
</Headers>
```

`JSON`格式

```
{
    "Result": [
        {
            "timestamp": 1552868172741,
            "host": "192.168.**.**",
            "contentCollection": {
                "level": "info",
                "host": "192.168.**.**",
                "time": "2019-03-18T08:16:12.741Z",
                "content": "[o.e.c.r.a.AllocationService] [MnNASM_] Cluster health status changed from [YELLOW] to [GREEN] (reason: [shards started [[my_index][3]] ...])."
            },
            "instanceId": "es-cn-n6w1o1x0w001c****"
        },
        {
            "timestamp": 1552838205462,
            "host": "192.168.**.**",
            "contentCollection": {
                "level": "info",
                "host": "192.168.**.**",
                "time": "2019-03-17T23:56:45.462Z",
                "content": "[o.e.c.r.a.AllocationService] [v4p9o7A] Cluster health status changed from [GREEN] to [YELLOW] (reason: [{MnNASM_}{MnNASM_OSR-2YgySSc****}{EvJHPrAOS_u8J3-6qZ****}{192.168.**.**}{192.168.**.**:9300}{ml.max_open_jobs=10, ml.enabled=true} transport disconnected])."
            },
            "instanceId": "es-cn-n6w1o1x0w001c****"
        }
    ],
    "RequestId": "121753D9-744A-4203-9EC4-F29E628A****",
    "Headers": {
        "X-Total-Count": 2
    }
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

