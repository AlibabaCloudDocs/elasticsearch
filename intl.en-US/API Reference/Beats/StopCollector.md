# StopCollector

Call StopCollector to stop the running collector.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=StopCollector&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     POST /openapi/collectors/[ResId]/actions/stop HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ResId|String|Path|Yes|ct-cn-77uqof2s7rg5c\*\*\*\*|The collector ID. |
|ClientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. The value of this parameter is generated by the client and is unique among different requests. The maximum length is 64 ASCII characters. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Indicates whether SQL audit was disabled for the DRDS database. |

## Examples

Sample requests

```

     POST /openapi/collectors/ct-cn-77uqof2s7rg5c **** /actions/stop HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": true, "RequestId": "7A29449F-A241-4A5D-92B5-B1ABA4F5****" } 
   
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

