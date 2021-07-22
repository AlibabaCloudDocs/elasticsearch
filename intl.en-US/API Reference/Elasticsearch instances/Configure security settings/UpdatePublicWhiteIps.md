# UpdatePublicWhiteIps

You can call this operation to UpdatePublicWhiteIps the public endpoint access whitelist of a specified Elasticsearch instance.

**Note:** When you call this operation, note that the public endpoint whitelist of the instance cannot be updated when the instance status is active, invalid, and inactive.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdatePublicWhiteIps&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     PATCH|POST /openapi/instances/[InstanceId]/public-white-ips HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-tl329rbpc0001\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|modifyMode|String|Query|No|Cover|The modification method. Valid values:

-   Cover \(default\): overwrites the original IP address whitelist with the value of the ips parameter.
-   Append: Add the IP addresses entered in the ips parameter to the original IP address whitelist.
-   Delete: Delete the IP addresses entered in the ips parameter from the original IP address whitelist. You must retain at least one IP address. |

## RequestBody

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|publicIpWhitelist

|List<String\>

|Yes

|\["0.0.0.0/0","0.0.0.0/1"\]

|The public whitelist of the instance. |
|whiteIpGroup

|Struct

|No

|\{"groupName": "test\_group\_name","ips": \["0.0.0.0","10.2.XX.XX"\]\}

|Update the whitelist security configurations of an instance by using whitelist group whitelist parameters. You can update only one whitelist group. |
|└ groupName

|String

|Required in whiteIpGroup

|test\_group\_name

|The group name of the whitelist group. |
|└ ips

|List<String\>

|Required in whiteIpGroup

|\["0.0.0.0", "10.2.XX.XX"\]

|The list of IP addresses in the whitelist group. |

**Note:**

If the modifyMode parameter is set to Cover, the whitelist group is deleted if ips is empty.

If the modifyMode parameter is set to Cover, a new whitelist group is created if groupName is not in the list of existing whitelist group names.

If modifyMode is set to Delete, the deleted ips must retain at least one IP address.

If modifyMode is set to Append, ensure that the whitelist group name is created. Otherwise, NotFound is reported.

The addition and deletion of a whitelist group is implemented by calling modifyMode to Cover. Delete and Append cannot add or delete the whitelist group granularity, and can only modify the IP list in the whitelist group.

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|C82758DD-282F-4D48-934F-92170A33\*\*\*\*|The ID of the request. |
|Result|Struct| |The returned results. |
|publicIpWhitelist|List|\["0.0.0.0","10.2.XX.XX","110.0.XX.XX/8"\]|The configuration of the public network whitelist. |

**Note:** In the following return example, this article only guarantees that the parameters in the return data list are included, and the parameters not mentioned are for reference only. For more information about the parameter description, see [ListInstance](~~142230~~). You cannot force a dependency in a program to obtain these parameters.

## Examples

Sample requests

```

     POST /openapi/instances/es-cn-tl329rbpc0001****/public-white-ips HTTP/1.1 { "publicIpWhitelist": [ "110.0.XX.XX/8" ] } or { "whiteIpGroup": { "groupName": "test_group_name", "ips": [ "0.0.0.0", "10.2.XX.XX" ] } } 
   
```

Sample success responses

`JSON` format

```

     { "Result": { "instanceId": "es-cn-tl329rbpc0001****", "version": "7.10.0_with_X-Pack", "description": "test", "nodeAmount": 0, "paymentType": "postpaid", "status": "active", "privateNetworkIpWhiteList": [ "11.22.XX.XX", "0.0.XX.XX/0" ], "enablePublic": true, "nodeSpec": {}, "dataNode": false, "networkConfig": { "vpcId": "vpc-bp1jy348ibzulk6h***", "vswitchId": "vsw-bp1a0mifpletdd1da****", "vsArea": "cn-hangzhou-h", "whiteIpGroupList": [ { "groupName": "default", "ips": [ "0.0.XX.XX/0", "11.22.XX.XX" ], "whiteIpType": "PRIVATE_ES" }, { "groupName": "default", "ips": [ "110.0.XX.XX/9" ], "whiteIpType": "PUBLIC_KIBANA" }, { "groupName": "default", "ips": [ "192.168.XX.XX/24" ], "whiteIpType": "PRIVATE_KIBANA" }, { "groupName": "default", "ips": [ "110.0.XX.XX/8" ], "whiteIpType": "PUBLIC_ES" }, { "groupName": "test_group_name", "ips": [ "0.0.0.0", "10.2.XX.XX" ], "whiteIpType": "PUBLIC_ES" } ], "type": "vpc" }, "createdAt": "2021-07-21T01:29:38.510Z", "updatedAt": "2021-07-21T06:40:32.438Z", "commodityCode": "elasticsearch", "extendConfigs": [ { "configType": "usageScenario", "value": "log" }, { "configType": "maintainTime", "maintainStartTime": "02:00Z", "maintainEndTime": "06:00Z" }, { "configType": "aliVersion", "aliVersion": "ali1.4.0" }, { "configType": "followCube", "followClusterEnabled": true } ], "endTime": 4782556800000, "clusterTasks": [], "vpcInstanceId": "es-cn-tl329rbpc0001****-worker", "resourceGroupId": "rg-acfmxxkk2p7****", "zoneCount": 1, "protocol": "HTTP", "zoneInfos": [ { "zoneId": "cn-hangzhou-h", "status": "NORMAL" } ], "instanceType": "elasticsearch", "inited": true, "tags": [ { "tagKey": "acs:rm:rgId", "tagValue": "rg-acfmxxkk2p7****" } ], "serviceVpc": true, "domain": "es-cn-tl329rbpc0001****.elasticsearch.aliyuncs.com", "port": 9200, "esVersion": "7.10.0_with_X-Pack", "esConfig": { "action.destructive_requires_name": "true", "xpack.watcher.enabled": "false", "action.auto_create_index": "+.*,-*" }, "esIPWhitelist": [ "11.22.XX.XX", "0.0.XX.XX/0" ], "esIPBlacklist": [], "kibanaProtocol": "HTTPS", "kibanaIPWhitelist": [ "::1", "110.0.XX.XX/9" ], "kibanaPrivateIPWhitelist": [ "192.168.XX.XX/24" ], "publicIpWhitelist": [ "0.0.0.0", "10.2.XX.XX", "110.0.XX.XX/8" ], "kibanaDomain": "es-cn-tl329rbpc0001****.kibana.elasticsearch.aliyuncs.com", "kibanaPort": 5601, "kibanaPrivateDomain": "es-cn-tl329rbpc0001****-kibana.internal.elasticsearch.aliyuncs.com", "kibanaPrivatePort": 5601, "publicPort": 9200, "publicDomain": "es-cn-tl329rbpc0001****.public.elasticsearch.aliyuncs.com", "haveKibana": true, "instanceCategory": "IS", "dedicateMaster": false, "advancedDedicateMaster": false, "masterConfiguration": {}, "haveClientNode": false, "warmNode": true, "warmNodeConfiguration": { "spec": "elasticsearch.d1.2xlarge", "amount": 3 }, "clientNodeConfiguration": {}, "kibanaConfiguration": { "spec": "elasticsearch.n4.small", "amount": 1, "disk": 0 }, "elasticDataNodeConfiguration": {}, "haveElasticDataNode": false, "dictList": [ { "name": "SYSTEM_MAIN.dic", "fileSize": 2782602, "sourceType": "ORIGIN", "type": "MAIN" }, { "name": "SYSTEM_STOPWORD.dic", "fileSize": 132, "sourceType": "ORIGIN", "type": "STOP" } ], "synonymsDicts": [], "ikHotDicts": [], "aliwsDicts": [], "haveGrafana": false, "haveCerebro": false, "enableKibanaPublicNetwork": true, "enableKibanaPrivateNetwork": true, "advancedSetting": { "gcName": "CMS" }, "enableMetrics": true, "readWritePolicy": { "writeHa": false } }, "RequestId": "9E5466DE-E29A-4F87-9019-4AA7B80E60DE" } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceActivating|Instance is activating.|The instance is currently in effect.|
|400|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

