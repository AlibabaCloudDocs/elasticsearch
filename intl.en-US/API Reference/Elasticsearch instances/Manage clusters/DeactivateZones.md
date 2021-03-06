# DeactivateZones

Call deactivatezones to offline part of the zone when multiple zones are available. And you need to migrate the nodes in the offline zone to other zones.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DeactivateZones&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/actions/down-zones HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|clientToken|String|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|A unique token generated by the client to guarantee the idempotency of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can only contain ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

You must also specify the IDs of the zones in the RequestBody field. Example:

`["cn-hangzhou-i","cn-hangzhou-f"]`

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   true: offline zone successfully
-   false: offline zone successfully failed |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/down-zones HTTP/1.1
Common request parameters
["cn-hangzhou-i","cn-hangzhou-f"]
```

Sample success responses

`JSON` format

```
{
    "Result": true,
    "RequestId": "5A5D8E74-565C-43DC-B031-29289FA****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

