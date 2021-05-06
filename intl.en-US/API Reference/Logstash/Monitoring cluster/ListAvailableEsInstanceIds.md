# ListAvailableEsInstanceIds

Call the ListAvailableEsInstanceIds to obtain the list of Elasticsearch instances\(with the X-Pack monitoring capability\) when you set X-Pack to monitor Logstash instances.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=ListAvailableEsInstanceIds&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/available-elasticsearch-for-centralized-management HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the Logstash instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|description|String|instanceName|The name of the Elasticsearch instance. |
|endpoint|String|http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200|The public endpoint of the Elasticsearch instance. |
|esInstanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the Elasticsearch instance. |
|kibanaEndpoint|String|https://es-cn-n6w1o1x0w001c\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|The public network endpoint of Kibana. |

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/available-elasticsearch-for-centralized-management HTTP/1.1
common request header
```

Sample success responses

`JSON` Syntax

```
{
    "Result": [
        {
            "esInstanceId": "es-cn-n6w1o1x0w001c****",
            "endpoint": "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200",
            "description": "pan_67_keepit",
            "kibanaEndpoint": "https://es-cn-n6w1o1x0w001c****.kibana.elasticsearch.aliyuncs.com:5601"
        },
        {
            "esInstanceId": "es-cn-6ja1rgego0006****",
            "endpoint": "http://es-cn-6ja1rgego0006****.elasticsearch.aliyuncs.com:9200",
            "description": "pan_mulitzones_keepit",
            "kibanaEndpoint": "https://es-cn-6ja1rgego0006****.kibana.elasticsearch.aliyuncs.com:5601"
        }
    ],
    "RequestId": "4047A0E1-DEE0-4C6C-92A9-E52D32FD****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

