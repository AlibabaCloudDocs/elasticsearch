# ValidateShrinkNodes

Calls ValidateShrinkNodes to check whether some nodes in a specified instance can be shrunken.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ValidateShrinkNodes&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/validate-shrink-nodes HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Required|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
|nodeType|String|Required|WORKER|The type of nodes. WORKER indicates hot node and WORKER\_WARM indicates warm node. |
|ignoreStatus|Boolean|No|false|Specifies whether to ignore the health status of the cluster. Default value: false. |

## RequestBody

You must enter the following parameters in RequestBody to specify the IP address and port number of the node to be verified:

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

-   true: nodes can be shrunken
-   false: nodes cannot be shrunken |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-nif1q9o8r0008****/validate-shrink-nodes? nodeType=WORKER HTTP/1.1
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
<RequestId>3760F67B-691D-4663-B4E5-6783554F****</RequestId>
```

`JSON` format

```
{
    "Result":true,
    "RequestId":"3760F67B-691D-4663-B4E5-6783554F****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

