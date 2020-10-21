# RenewInstance

Call RenewInstance to renew a subscription instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=RenewInstance&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/actions/renew HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|clientToken|String|No|5A2CFF0E-5718-45B5-9D4D-70B350\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can only contain ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

The following fields are also required in RequestBody to specify the renewal information.

|Field

|Type

|Required

|Example

|Description |
|-------|------|----------|---------|-------------|
|duration

|Integer

|Yes

|1

|The renewal duration of the subscription cluster. If pricingCycle is set to Year, the duration is 1 to 3. If pricingCycle is set to Month, the duration is 1 to 9. |
|pricingCycle

|String

|Yes

|Year

|The billing cycle of renewal. Valid values: Year and Month. |

Example:

```

{
    "duration":1,
    "pricingCycle":"Year"
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   true: renewal successfully
-   false: renewal failed |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/renew HTTP/1.1
Common request parameters
{
    "duration":1,
    "pricingCycle":"Year"
}
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>4FF74B95-7D01-44B4-8E0D-6E5AB515****</RequestId>
```

`JSON` format

```
{
    "Result": true,
    "RequestId": "4FF74B95-7D01-44B4-8E0D-6E5AB515****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

