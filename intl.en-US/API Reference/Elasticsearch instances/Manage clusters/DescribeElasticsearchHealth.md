# DescribeElasticsearchHealth

Call Describe Elasticsearch Health to obtain the health status of the specified Elasticsearch instance.

The instance health condition supports the following three states:

-   GREEN: The distribution of primary and secondary shards is normal.
-   YELLOW: The primary shard is normally allocated, but the replica is not normally allocated.
-   RED: The primary shard is not normally allocated.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeElasticsearchHealth&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/elasticsearch-health HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-tl325wxga000l\*\*\*\*|The ID of the request. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|200|The HTTP status code. |
|Message|String|success|The response message. |
|RequestId|String|0731F217-2C8A-4D42-8BCD-5C352866E3B7|The ID of the request. |
|Result|String|GREEN|The returned instance health status. |

## Examples

Sample requests

```

     GET /openapi/instances/[es-cn-tl325wxga000l ****]/elasticsearch-health HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": "GREEN", "RequestId": "0731F217-2C8A-4D42-8BCD-5C352866E3B7" } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceActivating|Instance is activating.|The instance is currently in effect.|
|404|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Please check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

