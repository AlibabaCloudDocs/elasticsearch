# DescribeConnectableClusters

Call DescribeConnectableClusters to query the instances that can communicate with the current instance. Interconnected instances are not covered.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeConnectableClusters&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers.

## Request syntax

```
GET /openapi/instances/[InstanceId]/connectable-clusters HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the current instance. |
|alreadySetItems|Boolean|No|true|Specifies whether to return the interconnected instances. The value of true is the default value, indicating that the returned instances are included in the list. The value of false indicates that the returned instances are not included. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of ConnectableClustersInfo| |The return results. |
|instances|String|es-cn-xxx|The ID of the instance for which network communication is available. |
|networkType|String|vpc|The network type of the instance. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/connectable-clusters HTTP/1.1
```

Sample success responses

`JSON` format

```
{
    "Result": [
        {
            "instanceId": "es-cn-09k1rgid9000g****",
            "networkType": "vpc"
        }
    ],
    "RequestId": "506B7495-0887-4F05-BC76-5AE7AC1D****"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

