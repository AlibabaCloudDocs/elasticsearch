# ValidateTransferableNodes

Call ValidateTransferableNodes to check whether the data on some nodes in the specified instance can be migrated.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ValidateTransferableNodes&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/validate-transfer-nodes HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
|nodeType|String|Yes|WORKER|The type of nodes.**WORKER**represents a hot node,**WORKER\_WARM**represents a warm node. |

## RequestBody

You must enter the following parameters in RequestBody to specify the IP address and port number of the node to be verified:

|Parameter

|Type

|Example

|Description |
|-----------|------|---------|-------------|
|host

|String

|172.16.xx.xx

|The IP address of the node. |
|port

|Integer

|9200

|The access port number of the node. |

Example:

```

[
    {
        "host": "172.16.**.**",
        "port": 9200
    },
    {
        "host": "172.16.**.**",
        "port": 9200
    }
]
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   Yes: the data can be migrated
-   No: the data can not be migrated |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-nif1q9o8r0008****/validate-transfer-nodes? nodeType=WORKER HTTP/1.1
Common request parameters
[
    {
        "host": "172.16. **.**",
        "port": 9200
    },
    {
        "host": "172.16. **.**",
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

