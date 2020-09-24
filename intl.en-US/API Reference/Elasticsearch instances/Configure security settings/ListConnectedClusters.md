# ListConnectedClusters

Call ListConnectedClusters to query the instances that are interconnected with the current instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListConnectedClusters&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

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
|Result|Array of Result| |The return results of the OCR task. |
|Result| | | |
|instances|String|es-cn-09k1rocex0006\*\*\*\*|The ID of the remote instance that is already communicating with the current instance. |
|networkType|String|vpc|The network type of the instance. |

## Examples

Sample requests

```
GET /openapi/instances/[InstanceId]/connected-clusters HTTP/1.1
Common request parameters
{
"InstanceId": "es-cn-0pp1jxvcl000z****"
}
```

Sample success responses

`XML` format

```
<Result>
    <instanceId>es-cn-09k1rocex0006****</instanceId>
    <networkType>vpc</networkType>
</Result>
<RequestId>8D6EE77E-D56E-4E88-A8EE-407B77FB****</RequestId>
```

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

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

