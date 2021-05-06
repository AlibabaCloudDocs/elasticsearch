# DescribeXpackMonitorConfig

Call the DescribeXpackMonitorConfig to obtain the X-Pack monitoring configurations of a Logstash instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=DescribeXpackMonitorConfig&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/xpack-monitor-config HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the Logstash instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The returned results. |
|enable|Boolean|true|Indicates whether to enable X-Pack monitoring. Valid values:

-   true: enabled
-   false: disabled |
|endpoints|List|\["http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|X-Pack list of access addresses for the Elasticsearch instance associated with the monitoring. |
|esInstanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the X-Pack instance associated with the Elasticsearch monitoring task. |
|pipelineIds|List|\[\]|X-Pack list of pipelines managed by the associated Kibana instance for monitoring. |
|userName|String|elastic|X-Pack username that is used to access the Elasticsearch instance associated with the monitoring job. |

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/xpack-monitor-config HTTP/1.1 
common request header
```

Sample success responses

`JSON` Syntax

```
{
    "Result": {
        "esInstanceId": "es-cn-n6w1o1x0w001c****",
        "endpoints": [
            "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
        ],
        "pipelineIds": [],
        "userName": "elastic",
        "enable": true
    },
    "RequestId": "9EC7377A-60D7-4AB2-ADE3-983E1D0D****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

