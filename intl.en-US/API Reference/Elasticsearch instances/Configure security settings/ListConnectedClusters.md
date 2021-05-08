# ListConnectedClusters

Call ListConnectedClusters to query the instances that are interconnected with the current instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListConnectedClusters&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/connected-clusters HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-0pp1jxvcl000z\*\*\*\*|The ID of the current instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|Result| | | |
|instances|String|es-cn-09k1rocex0006\*\*\*\*|The ID of the remote instance that is connected to the network of the current instance. |
|networkType|String|vpc|The network type of the instance. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-0pp1jxvcl000z****/connected-clusters HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": [
        {
            "instanceId": "es-cn-09k1rocex0006****",
            "networkType": "vpc"
        }
    ],
    "RequestId": "8D6EE77E-D56E-4E88-A8EE-407B77FB****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

