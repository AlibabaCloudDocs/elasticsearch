# DescribePipelineManagementConfig

Call the DescribePipelineManagementConfig to obtain the Logstash pipeline management configuration.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=DescribePipelineManagementConfig&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/pipeline-management-config HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the Logstash instance. |
|clientToken|String|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|A unique token generated by the client to guarantee the idempotency of the request. The value of this parameter is generated by the client and is unique among different requests. The maximum length is 64 ASCII characters. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|endpoints|String|\["http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|The list of access addresses of the Elasticsearch instance. Format: `Domains: port number`. |
|esInstanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the Elasticsearch instance. |
|pipelineIds|List|\["testKibanaManagement"\]|The list of pipeline names. |
|pipelineManagementType|String|MULTIPLE\_PIPELINE|Pipeline management method. Supports Kibana and MULTIPLE\_PIPELINE. |
|userName|String|elastic|The username that is used to access the instance. |

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/pipeline-management-config HTTP/1.1 
common request header 
```

Sample success responses

`JSON` Syntax

```
{
    "Result": {
        "pipelineManagementType": "MULTIPLE_PIPELINE",
        "esInstanceId": "es-cn-n6w1o1x0w001c****",
        "endpoints": [
            "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
        ],
        "pipelineIds": [
            "testKibanaManagement"
        ],
        "userName": "elastic"
    },
    "RequestId": "6822F07C-A896-4A2C-A430-BC01D5D1****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the instance cannot be found. Check the status of the instance.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).
