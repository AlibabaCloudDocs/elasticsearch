# ModifyWhiteIps

Updates the IP address whitelist of a specified Elasticsearch cluster.

**Note:** When you call this operation, note that the system cannot update the information when the instance status is active, invalid, and inactive.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ModifyWhiteIps&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     PATCH|POST /openapi/instances/[InstanceId]/actions/modify-white-ips HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-0pp1jxvcl000z\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|nodeType|String|Body|No|KIBANA|The instance type of the node. Valid values: WORKER\(Elasticsearch cluster\) and KIBANA\(Kibana cluster\). This parameter is required if the whiteIpList parameter is optional. |
|networkType|String|Body|No|PRIVATE|The network type of the cluster. Valid values: PRIVATE \(private network\) and PUBLIC \(public network\). This parameter is required if the whiteIpList parameter is optional. |
|modifyMode|String|Body|No|Cover|The modification method. Valid values:

-   Cover \(default\): overwrites the original IP address whitelist with the value of the ips parameter.
-   Append: Add the IP addresses entered in the ips parameter to the original IP address whitelist.
-   Delete:Delete:Delete the IP addresses entered in the ips parameter from the original IP address whitelist. You must retain at least one IP address. |
|whiteIpList|List<String\>|Body|No|\["0.0.0.0/0","0.0.0.0/1"\]|The list of IP addresses in the whitelist. |
|whiteIpGroup|Struct|Body|No|\{ "groupName": "test\_group\_name", "ips": \[ "0.0.0.0", "10.2.XX.XX" \] \}|Update the whitelist security configurations of an instance by using whitelist group whitelist parameters. You can update only one whitelist group. |
|groupName|String| |No|test\_group\_name|The group name of the whitelist group. This parameter is required if the whiteIpGroup parameter is optional. |
|ips|String| |No|\["0.0.0.0", "10.2.XX.XX"\]|The list of IP addresses in the whitelist group. This parameter is required if the whiteIpGroup parameter is optional. |

**Note:**

If the modifyMode parameter is set to Cover, the whitelist group is deleted if ips is empty.

If the modifyMode parameter is set to Cover, a new whitelist group is created if groupName is not in the list of existing whitelist group names.

If modifyMode is set to Delete, the deleted ips must retain at least one IP address.

If modifyMode is set to Append, ensure that the whitelist group name is created. Otherwise, NotFound is reported.

The addition and deletion of a whitelist group is implemented by calling modifyMode to Cover. Delete and Append cannot add or delete the whitelist group granularity, and can only modify the IP list in the whitelist group.

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Sample response:

-   true: The whitelist is updated.
-   false: The whitelist fails to be updated. |

The following returned parameter information is guaranteed to contain only the parameter information mentioned in the above returned parameter table, and the parameter information not mentioned is for reference only, and the program cannot be forced to rely on these parameters.

## Examples

Sample requests

```

     PATCH /openapi/instances/es-cn-0pp1jxvcl000z****/actions/modify-white-ips HTTP/1.1 { "nodeType":"KIBANA", "networkType":"PRIVATE", "whiteIpList": ["0.0.0.0/0","0.0.0.0/1"] } or { "nodeType":"KIBANA", "networkType":"PRIVATE", "whiteIpGroup": { "groupName": "test_group_name", "ips": [ "0.0.0.0", "10.2.XX.XX" ] } } 
   
```

Sample success responses

`JSON` format

```

     { "Result": true, "RequestId": "4EB052CA-17A1-4D45-A932-E1F08C7964CB" } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

