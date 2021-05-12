# StartCollector

Call StartCollector to start the collector.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=StartCollector&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     POST /openapi/collectors/[ResId]/actions/start HTTP/1.1 
   
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

     POST /openapi/collectors/ct-cn-77uqof2s7rg5c **** /actions/start HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": true, "RequestId": "75B1A9CE-F8D5-4BA2-9D24-E92137C6****" } 
   
```

## Error codes

Visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.
