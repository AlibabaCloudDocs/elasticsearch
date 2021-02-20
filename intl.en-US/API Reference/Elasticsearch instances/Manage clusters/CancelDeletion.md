# CancelDeletion

You can call the CancelDeletion operation to restore the released Elasticsearch instance that is frozen.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=CancelDeletion&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/actions/cancel-deletion HTTP/1.1 
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-z2q1wk6z00007\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|A unique token generated by the client to guarantee the idempotency of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|D682B6B3-B425-46DA-A5FC-5F5C60553622|The ID of the request. |
|Result|Boolean|true|Indicates whether the restoration of the instance is successful. Valid values:

-   true
-   false |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-z2q1wk6z00007****/actions/cancel-deletion HTTP/1.1 
common request header 
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>D682B6B3-B425-46DA-A5FC-5F5C60553622</RequestId>
```

`JSON` Syntax

```
{
  "Result": true,
  "RequestId": "D682B6B3-B425-46DA-A5FC-5F5C60553622"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the instance cannot be found. Check the status of the instance.|
