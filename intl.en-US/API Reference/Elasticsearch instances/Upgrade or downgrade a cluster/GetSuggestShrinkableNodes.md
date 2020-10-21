# GetSuggestShrinkableNodes

Call GetSuggestShrinkableNodes to specify the type and number of nodes to obtain the nodes that can be removed.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=GetSuggestShrinkableNodes&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/suggest-shrinkable-nodes HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|count|Integer|Yes|1|The number of nodes that you want to remove. |
|InstanceId|String|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
|nodeType|String|Yes|WORKER|The type of removing nodes. WORKER indicates hot node and WORKER\_WARM indicates warm node. |
|ignoreStatus|Boolean|No|false|Specifies whether to ignore the instance status. Default value: false. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|host|String|192.168.\*\*. \*\*|The IP address of the node. |
|port|Integer|9200|The access port number of the node. |

The Result also contains the following parameters.

|Parameter

|Type

|Example

|Description |
|-----------|------|---------|-------------|
|nodeType

|String

|WORKER

|The type of the node. Valid values: MASTER \(dedicated MASTER node\), WORKER \(hot node\), WORKER\_WARM \(warm node\), COORDINATING \(client node\), and KIBANA\(Kibana node\). |
|zoneId

|String

|cn-hangzhou-b

|The ID of the zone to which the node belongs. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/suggest-shrinkable-nodes? nodeType=WORKER&count=1 HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <nodeType>WORKER</nodeType>
    <host>172.16. **.**</host>
    <port>9200</port>
    <zoneId>cn-hangzhou-i</zoneId>
</Result>
<RequestId>042E33B2-6FB3-474D-BD44-DBE706A4****</RequestId>
```

`JSON` format

```
{
    "Result": [
        {
            "nodeType": "WORKER",
            "host": "172.16. **.**",
            "port": 9200,
            "zoneId": "cn-hangzhou-i"
        }
    ],
    "RequestId": "042E33B2-6FB3-474D-BD44-DBE706A4****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

