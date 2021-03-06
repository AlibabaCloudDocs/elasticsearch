# CloseHttps

Call CloseHttps to close the HTTPS protocol.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=CloseHttps&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/actions/close-https HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|clientToken|String|No|5A2CFF0E-5718-45B5-9D4D-70B350\*\*\*\*|A unique token generated by the client to guarantee the idempotency of the request. You can use the client to generate a token, but you must ensure that it is unique among different requests. The token can only contain ASCII characters and cannot exceed 64 characters in length. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DC\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   true: HTTPS protocol closed successfully
-   false: HTTPS protocol closed failed |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/close-https HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": true,
    "RequestId": "5A5D8E74-565C-43DC-B031-29289FA*****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

