# ListLogstashLog

Call the ListLogstashLog to view the logs of the Logstash instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=ListLogstashLog&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/search-log HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|beginTime|Long|Yes|1531910852074|The start timestamp of the log. Unit: milliseconds. |
|endTime|Long|Yes|1531910852074|The timestamp when the log entry ended. Unit: milliseconds. |
|InstanceId|String|Yes|ls-cn-v0h1kzca\*\*\*\*|The ID of the instance. |
|page|Integer|Yes|1|The page number of the returned page. Minimum value: 1. Default value: 1. |
|query|String|Yes|host:10.7.xx.xx AND level:info AND content:opening|The keyword used to match log entries. |
|size|Integer|Yes|20|The number of entries to return on each page. |
|type|String|Yes|LOGSTASH\_INSTANCE\_LOG|The type of the logs. Valid values: LOGSTASH\_INSTANCE\_LOG \(main log\), SEARCHSLOW\(searching slow log\), INDEXINGSLOW\(indexing slow log\), JMVLOG\(GC log\), and LOGSTASH\_DEBUG\_LOG \(debugging log\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|7F40EAA1-6F1D-4DD9-8DB8-C5F00C4E\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|content|String|\[logstash.outputs.fileextend\] Opening file \{:path=\>\\"/ssd/1/ls-cn-v0h1kzca\*\*\*\*/logstash/logs/debug/test\\"\}|The details of the log. |
|host|String|192.168.xx.xx|The IP address of the node that generates the log entry. |
|instanceId|String|ls-cn-v0h1kzca\*\*\*\*|The ID of the Elasticsearch instance. |
|level|String|info|The level of the log entry. Including trace, debug, info, warn, error and so on \(GC log has no level\). |
|timestamp|Long|1531985112420|The timestamp when the log is generated. Unit: milliseconds. |

The returned data also contains the following parameters.

|Parameter

|Type

|Example

|Description |
|-----------|------|---------|-------------|
|Result

|Struct

| |The return results. |
|└time

|String

|2020-07-22T16:58:00.506Z

|The time when the log entry was generated. |
|Headers

|Struct

| |The header of the response. |
|└X-Total-Count

|Integer

|1

|The number of logs to return. |

**Note:** └ indicates a child parameter.

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-v0h1kzca****/search-log?type=LOGSTASH_INSTANCE_LOG&query=host:10.7.xx.xx AND level:info AND content:opening&beginTime=1531910852074&endTime=1531910852074&page=1&size=20 HTTP/1.1
common request header
```

Sample success responses

`JSON` Syntax

```
{
    "Result": [
        {
            "timestamp": 1595408280506,
            "host": "10.7.**.**",
            "contentCollection": {
                "level": "info",
                "host": "10.7.**.**",
                "time": "2020-07-22T16:58:00.506Z",
                "content": "[logstash.outputs.fileextend] Opening file {:path=>\"/ssd/1/ls-cn-v0h1kzca****/logstash/logs/debug/test\"}"
            },
            "instanceId": "ls-cn-v0h1kzca****"
        }
    ],
    "RequestId": "DADBEFD2-570D-48EE-ABE4-0E3017D8****",
    "Headers": {
        "X-Total-Count": 1
    }
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the instance cannot be found. Check the status of the instance.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

