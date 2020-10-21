# ListSearchLog

Call ListSearchLog to view instance logs.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListSearchLog&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/search-log HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|beginTime|Long|Yes|1531910852074|The beginning timestamp of the log. Unit: ms. |
|endTime|Long|Yes|1531910852074|The endding timestamp of the log. Unit: ms. |
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|page|Integer|Yes|1|The page number of the plug-in list. Minimum value: 1. Default value: 1. |
|query|String|Yes|host:172.16. \*\*. \*\* AND content:netty|The keywords to query. |
|size|Integer|Yes|20|The number of entries to return on each page. |
|type|String|Yes|INSTANCELOG|The type of the logs. Valid values:

-   **INSTANCELOG**: Cluster log
-   **SEARCHSLOW**: Search slow log
-   **INDEXINGSLOW**: Indexing slow log
-   **JMVLOG**: GC log
-   **ES\_SEARCH\_ACCESS\_LOG**: Access log |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|1000|The number of entries returned per page. |
|RequestId|String|7F40EAA1-6F1D-4DD9-8DB8-C5F00C4E\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The list of returned logs. |
|content|String|\[GC \(Allocation Failure\) 2018-07-19T17:24:20.682+0800: 7516.513: \[ParNew: 6604768K-\>81121K\(7341504K\), 0.0760606 secs\] 7226662K-\>703015K\(31813056K\), 0.0762507 secs\] \[Times: user=0.52 sys=0.00, real=0.07 secs\]|The content of the log entry. |
|host|String|192.168.\*\*. \*\*|The IP address of the node that generates the log. |
|instanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the Elasticsearch instance. |
|level|String|info|The level of the log entry. Valid values:

-   warn: warning logs
-   info: Information logs
-   error: error logs
-   trace: trace logs
-   debug: debug logs |
|timestamp|Long|1531985112420|The timestamp when the log is generated. Unit: ms. |

The Result also contains the following parameters.

|Parameter

|Type

|Example

|Description |
|-----------|------|---------|-------------|
|time

|String

|2020-07-21T11:12:53.057Z

|The time when the log entry was generated. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/search-log? type=INSTANCELOG&query=host:172.16. **. ** AND content:netty&beginTime=1531910852074&endTime=1531910852074&page=1&size=20 HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <timestamp>1552868172741</timestamp>
    <host>192.168. **.**</host>
    <contentCollection>
        <level>info</level>
        <host>192.168. **.**</host>
        <time>2019-03-18T08:16:12.741Z</time>
        <content>[o.e.c.r.a.AllocationService] [MnNASM_] Cluster health status changed from [YELLOW] to [GREEN] (reason: [shards started [[my_index][3]] ...]).</content>
    </contentCollection>
    <instanceId>es-cn-n6w1o1x0w001c****</instanceId>
</Result>
<Result>
    <timestamp>1552838205462</timestamp>
    <host>192.168. **.**</host>
    <contentCollection>
        <level>info</level>
        <host>192.168. **.**</host>
        <time>2019-03-17T23:56:45.462Z</time>
        <content>[o.e.c.r.a.AllocationService] [v4p9o7A] Cluster health status changed from [GREEN] to [YELLOW] (reason: [{MnNASM_}{MnNASM_OSR-2YgySSc****}{EvJHPrAOS_u8J3-6qZ****}{192.168. **. **}{192.168. **.**:9300}{ml.max_open_jobs=10, ml.enabled=true} transport disconnected]).</content>
    </contentCollection>
    <instanceId>es-cn-n6w1o1x0w001c****</instanceId>
</Result>
<RequestId>121753D9-744A-4203-9EC4-F29E628A****</RequestId>
<Headers>
    <X-Total-Count>2</X-Total-Count>
</Headers>
```

`JSON` format

```
{
    "Result": [
        {
            "timestamp": 1552868172741,
            "host": "192.168. **.**",
            "contentCollection": {
                "level": "info",
                "host": "192.168. **.**",
                "time": "2019-03-18T08:16:12.741Z",
                "content": "[o.e.c.r.a.AllocationService] [MnNASM_] Cluster health status changed from [YELLOW] to [GREEN] (reason: [shards started [[my_index][3]] ...])."
            },
            "instanceId": "es-cn-n6w1o1x0w001c****"
        },
        {
            "timestamp": 1552838205462,
            "host": "192.168. **.**",
            "contentCollection": {
                "level": "info",
                "host": "192.168. **.**",
                "time": "2019-03-17T23:56:45.462Z",
                "content": "[o.e.c.r.a.AllocationService] [v4p9o7A] Cluster health status changed from [GREEN] to [YELLOW] (reason: [{MnNASM_}{MnNASM_OSR-2YgySSc****}{EvJHPrAOS_u8J3-6qZ****}{192.168. **. **}{192.168. **.**:9300}{ml.max_open_jobs=10, ml.enabled=true} transport disconnected])."
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

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

