# TransferNode

Call TransferNode to run a data migration task.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=TransferNode&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/actions/transfer HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
|nodeType|String|Yes|WORKER|The type of nodes.**WORKER**represents a hot node,**WORKER\_WARM**represents a warm node. |
|clientToken|String|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|A unique token generated by the client to guarantee the idempotency of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can only contain ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

You must also enter the following parameters in RequestBody to specify the IP address and port number of the node to be migrated.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|host

|String

|Yes

|192.168.xx.xx

|The IP address of the node. |
|port

|Integer

|Yes

|9200

|The access port number of the node. |

Example:

```

[
    {
        "host": "192.168. **.**",
        "port": 9200
    },
    {
        "host": "192.168. **.**",
        "port": 9200
    }
]
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   true: task execution successfully
-   false: task execution failed |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-nif1q9o8r0008****/actions/transfer? nodeType=WORKER HTTP/1.1
Common request parameters
[
    {
        "host": "192.168. **.**",
        "port": 9200
    },
    {
        "host": "192.168. **.**",
        "port": 9200
    }
]
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>C82758DD-282F-4D48-934F-92170A33****</RequestId>
```

`JSON` format

```
{
    "Result": true,
    "RequestId": "C82758DD-282F-4D48-934F-92170A33****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

