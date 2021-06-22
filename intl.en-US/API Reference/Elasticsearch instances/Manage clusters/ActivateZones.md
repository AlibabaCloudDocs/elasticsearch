# ActivateZones

call ActivateZones, restore offline zone. This operation is available only for multi-zone Elasticsearch clusters.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ActivateZones&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters. For more information, see the Common request parameters topic.

## Request syntax

```

     POST /openapi/instances/[InstanceId]/actions/recover-zones HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Location|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The IDs of the added ECS instances. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

In the RequestBody, you also need to enter the list of available zone IDs to be restored, as shown in the following example.

`["cn-hangzhou-i","cn-hangzhou-h"]`

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5A5D8E74-565C-43DC-B031-29289FA\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Sample response:

-   true: specifies to recovery zone success
-   false: recovery zone failure |

## Examples

Sample requests

```

     POST /openapi/instances/es-cn-n6w1o1x0w001c **** /actions/recover-zones HTTP/1.1 public request header ["cn-hangzhou-i","cn-hangzhou-h"] 
   
```

Sample success responses

`JSON` format

```

     { "Result": true, "RequestId": "5A5D8E74-565C-43DC-B031-29289FA****" } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Please check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

