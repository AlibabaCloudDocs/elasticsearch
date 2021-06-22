# UpdateDescription

Changes the name of a specified Elasticsearch cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateDescription&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters. For more information, see the Common request parameters topic.

## Request syntax

```

     PATCH|POST /openapi/instances/[InstanceId]/description HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Location|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1ptcb30009\*\*\*\*|The IDs of the added ECS instances. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B350\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|description|String|Body|No|aliyunes\_name\_test|Specify the updated instance name. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|FDF34727-1664-44C1-A8DA-3EB72D60\*\*\*\*|The ID of the request. |
|Result|Struct| |The returned results. |
|description|String|aliyunes\_test\_name|The new name of the instance. |

**Note:** In the following return example, this article is guaranteed to contain only the parameters in the return data list, and the parameters not mentioned are for reference only. [ListInstance](~~142230~~) . The program cannot force to rely on obtaining these parameters.

## Examples

Sample requests

```

     PATCH /openapi/instances/es-cn-n6w1ptcb30009****/description?description=aliyunes_name_test HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": { "instanceId": "es-cn-n6w1ptcb30009****", "version": "5.5.3_with_X-Pack", "description": "aliyunes_name_test", "nodeAmount": 3, "paymentType": "postpaid", "status": "activating", "privateNetworkIpWhiteList": [ "0.0.0.0/0" ], "enablePublic": true, "nodeSpec": { "spec": "elasticsearch.n4.small", "disk": 40, "diskType": "cloud_ssd", "diskEncryption": false }, "networkConfig": { "vpcId": "vpc-bp16k1dvzxtmagcva****", "vswitchId": "vsw-bp1k4ec6s7sjdbudw****", "vsArea": "cn-hangzhou-i", "type": "vpc" }, "createdAt": "2020-06-28T08:25:52.895Z", "updatedAt": "2020-07-06T09:21:11.615Z", "commodityCode": "elasticsearch", "extendConfigs": [ { "configType": "usageScenario", "value": "general" }, { "configType": "maintainTime", "maintainStartTime": "02:00Z", "maintainEndTime": "06:00Z" } ], "endTime": 4749724800000, "clusterTasks": [ { "type": "updating", "progress": 59.84375, "status": "RUNNING", "canCancelable": false, "interruptible": true, "subTasks": [ { "type": "ecs", "progress": 100, "detail": { "totalNodeCount": 4, "completedNodeCount": 4 }, "status": "FINISHED", "canCancelable": false, "interruptible": false, "subTasks": [] }, { "type": "hippo", "progress": 100, "detail": {}, "status": "FINISHED", "canCancelable": false, "interruptible": false, "subTasks": [] }, { "type": "rolling", "progress": 39.375, "detail": { "totalNodeCount": 4, "completedNodeCount": 1 }, "status": "RUNNING", "canCancelable": false, "interruptible": false, "subTasks": [] }, { "type": "finally", "progress": 0, "detail": {}, "status": "READY", "canCancelable": false, "interruptible": false, "subTasks": [] } ] } ], "vpcInstanceId": "es-cn-n6w1ptcb30009****-worker", "resourceGroupId": "rg-acfm2h5vbzd****", "zoneCount": 1, "protocol": "HTTP", "zoneInfos": [ { "zoneId": "cn-hangzhou-i", "status": "NORMAL" } ], "instanceType": "elasticsearch", "inited": true, "tags": [], "domain": "es-cn-n6w1ptcb30009****.elasticsearch.aliyuncs.com", "port": 9200, "esVersion": "5.5.3_with_X-Pack", "esConfig": { "action.destructive_requires_name": "true", "xpack.security.audit.outputs": "index", "xpack.watcher.enabled": "false", "xpack.security.audit.enabled": "false", "action.auto_create_index": "true" }, "esIPWhitelist": [ "0.0.0.0/0" ], "esIPBlacklist": [], "kibanaIPWhitelist": [ "0.0.0.0/0", "::/0" ], "kibanaPrivateIPWhitelist": [], "publicIpWhitelist": [ "::1", "0.0.0.0/0" ], "kibanaDomain": "es-cn-n6w1ptcb30009****.kibana.elasticsearch.aliyuncs.com", "kibanaPort": 5601, "publicPort": 9200, "publicDomain": "es-cn-n6w1ptcb30009****.public.elasticsearch.aliyuncs.com", "haveKibana": true, "instanceCategory": "x-pack", "dedicateMaster": false, "advancedDedicateMaster": false, "masterConfiguration": {}, "haveClientNode": false, "warmNode": false, "warmNodeConfiguration": {}, "clientNodeConfiguration": {}, "kibanaConfiguration": { "spec": "elasticsearch.n4.small", "amount": 1, "disk": 0 }, "dictList": [ { "name": "SYSTEM_MAIN.dic", "fileSize": 3058510, "sourceType": "ORIGIN", "type": "MAIN" }, { "name": "SYSTEM_STOPWORD.dic", "fileSize": 164, "sourceType": "ORIGIN", "type": "STOP" } ], "synonymsDicts": [], "ikHotDicts": [], "aliwsDicts": [], "haveGrafana": false, "haveCerebro": false, "enableKibanaPublicNetwork": true, "enableKibanaPrivateNetwork": false, "advancedSetting": { "gcName": "CMS" } }, "RequestId": "24A68E4C-94B4-45E4-9068-CB12F33C****" } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Please check the instance status.|

For a list of error codes, visit the [API Error Center .](https://error-center.alibabacloud.com/status/product/elasticsearch)

