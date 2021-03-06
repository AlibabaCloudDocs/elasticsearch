# UpdateXpackMonitorConfig

Call the UpdateXpackMonitorConfig to update the X-Pack monitoring alert configurations of a Logstash instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=UpdateXpackMonitorConfig&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
POST /openapi/logstashes/[InstanceId]/xpack-monitor-config HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the Logstash instance. |
|ClientToken|String|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|A unique token generated by the client to guarantee the idempotency of the request. The value of this parameter is generated by the client and is unique among different requests. The maximum length is 64 ASCII characters. |

## RequestBody

Enter the following parameters in RequestBody to specify the monitoring configuration for X-Pack.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|endpoints

|List<String\\\>

|Yes

|http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200

|The access address of the Elasticsearch instance. |
|pipelineIds

|List<String\\\>

|Yes

|\["name-1","name-2"\]

|The ID List of Logstash pipelines to be monitored. |
|enable

|Boolean

|Yes

|true

|Specifies whether to enable X-Pack monitoring. |
|userName

|String

|Yes

|elastic

|Username of the Elasticsearch instance. |
|password

|String

|Yes

|\*\*\*\*

|Password of the Elasticsearch instance. |

Sample code:

```
{
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "pipelineIds": ["datahub_test","test"],
    "enable": true,
    "userName": "elastic",
    "password": "xxxx",
    "esInstanceId": "es-cn-n6w1o1x0w001c****"
}
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Boolean|true|Returned results:

-   true: update successful
-   false: update failed |

## Examples

Sample requests

```
POST /openapi/logstashes/ls-cn-oew1qbgl****/xpack-monitor-config HTTP/1.1
common request header
{
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "pipelineIds": ["datahub_test","test"],
    "enable": true,
    "userName": "elastic",
    "password": "xxxx",
    "esInstanceId": "es-cn-n6w1o1x0w001c****"
}
```

Sample success responses

`JSON` Syntax

```
{
    "Result": true,
    "RequestId": "30A59FC7-609B-4C12-B6EF-991A5CA7****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

