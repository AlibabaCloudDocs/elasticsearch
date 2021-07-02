# ModifyWhiteIps

Updates the IP address whitelist of a specified Elasticsearch cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ModifyWhiteIps&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     PATCH|POST /openapi/instances/[InstanceId]/actions/modify-white-ips HTTP/1.1 
   
```

## Request parameters

|Attribute|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-0pp1jxvcl000z\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|nodeType|String|Body|No|KIBANA|The type of the cluster. Valid values: Optional values: WORKER\(Elasticsearch cluster\) and KIBANA\(Kibana cluster\). |
|networkType|String|Body|No|PRIVATE|The network type of the cluster. Valid values: Optional values: PRIVATE \(private network\) and PUBLIC \(public network\). |
|whiteIpList|List<String\>|Body|No|\["0.0.0.0/0","0.0.0.0/1"\]|The whitelist of IP addresses. |

## Response parameters

|Attribute|Type|Sample response|Description|
|---------|----|---------------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Sample response:

-   true: specifies to whitelist updated successfully
-   false: whitelist update failed |

## Examples

Sample requests

```

     PATCH /openapi/instances/es-cn-0pp1jxvcl000z **** /actions/modify-white-ips HTTP/1.1 for more information about public request header {"whiteIpList": [ "0.0.0.0/0", "0.0.0.0/1" ], "nodeType":"WORKER", "networkType":"PUBLIC" } 
   
```

Sample success responses

`JSON` format

```

     { "Result": true, "RequestId": "EB03B04E-4700-4EC2-A4D6-9B623F09****" } 
   
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

