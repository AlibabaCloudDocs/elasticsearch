# DeleteVpcEndpoint

call DeleteVpcEndpoint, delete a service under the vpc of the terminal node.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DeleteVpcEndpoint&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     DELETE /openapi/instances/[InstanceId]/vpc-endpoints/[EndpointId] HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|EndpointId|String|Path|Yes|ep-bp18s6wy9420wdi4\*\*\*\*|The ID of the endpoint that you want to delete. |
|InstanceId|String|Path|Yes|es-cn-2r429tctl000d\*\*\*\*|The ID of the instance. |
|ClientToken|String|Query|No|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|A unique token generated by the client to ensure the idempotency of the request. You can use the client to generate a token, but you must make sure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC47D9|The ID of the request. |
|Result|Boolean|true|Indicates whether the deletion is successful. Valid values:

-   true: successfully deleted.
-   flase: The deletion failed. |

## Examples

Sample requests

```

     DELETE /openapi/instances/es-cn-2r429tctl000d****/vpc-endpoints/ep-bp18s6wy9420wdi4**** HTTP/1.1 
   
```

Sample success responses

`JSON` format

```

     { "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC47D9", "Result": true } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).
