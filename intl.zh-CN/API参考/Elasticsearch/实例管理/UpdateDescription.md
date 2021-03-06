# UpdateDescription

调用UpdateDescription，更新指定实例的名称。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateDescription&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/description HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1ptcb30009\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B350\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|description|String|Body|否|aliyunes\_name\_test|指定更新后的实例名称。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|FDF34727-1664-44C1-A8DA-3EB72D60\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|description|String|aliyunes\_test\_name|修改后的实例名称。 |

**说明：** 以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，参数说明可参见[ListInstance](~~142230~~)。程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-n6w1ptcb30009****/description?description=aliyunes_name_test HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"instanceId": "es-cn-n6w1ptcb30009****",
		"version": "5.5.3_with_X-Pack",
		"description": "aliyunes_name_test",
		"nodeAmount": 3,
		"paymentType": "postpaid",
		"status": "activating",
		"privateNetworkIpWhiteList": [
			"0.0.0.0/0"
		],
		"enablePublic": true,
		"nodeSpec": {
			"spec": "elasticsearch.n4.small",
			"disk": 40,
			"diskType": "cloud_ssd",
			"diskEncryption": false
		},
		"networkConfig": {
			"vpcId": "vpc-bp16k1dvzxtmagcva****",
			"vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
			"vsArea": "cn-hangzhou-i",
			"type": "vpc"
		},
		"createdAt": "2020-06-28T08:25:52.895Z",
		"updatedAt": "2020-07-06T09:21:11.615Z",
		"commodityCode": "elasticsearch",
		"extendConfigs": [
			{
				"configType": "usageScenario",
				"value": "general"
			},
			{
				"configType": "maintainTime",
				"maintainStartTime": "02:00Z",
				"maintainEndTime": "06:00Z"
			}
		],
		"endTime": 4749724800000,
		"clusterTasks": [
			{
				"type": "updating",
				"progress": 59.84375,
				"status": "RUNNING",
				"canCancelable": false,
				"interruptible": true,
				"subTasks": [
					{
						"type": "ecs",
						"progress": 100,
						"detail": {
							"totalNodeCount": 4,
							"completedNodeCount": 4
						},
						"status": "FINISHED",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					},
					{
						"type": "hippo",
						"progress": 100,
						"detail": {},
						"status": "FINISHED",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					},
					{
						"type": "rolling",
						"progress": 39.375,
						"detail": {
							"totalNodeCount": 4,
							"completedNodeCount": 1
						},
						"status": "RUNNING",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					},
					{
						"type": "finally",
						"progress": 0,
						"detail": {},
						"status": "READY",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					}
				]
			}
		],
		"vpcInstanceId": "es-cn-n6w1ptcb30009****-worker",
		"resourceGroupId": "rg-acfm2h5vbzd****",
		"zoneCount": 1,
		"protocol": "HTTP",
		"zoneInfos": [
			{
				"zoneId": "cn-hangzhou-i",
				"status": "NORMAL"
			}
		],
		"instanceType": "elasticsearch",
		"inited": true,
		"tags": [],
		"domain": "es-cn-n6w1ptcb30009****.elasticsearch.aliyuncs.com",
		"port": 9200,
		"esVersion": "5.5.3_with_X-Pack",
		"esConfig": {
			"action.destructive_requires_name": "true",
			"xpack.security.audit.outputs": "index",
			"xpack.watcher.enabled": "false",
			"xpack.security.audit.enabled": "false",
			"action.auto_create_index": "true"
		},
		"esIPWhitelist": [
			"0.0.0.0/0"
		],
		"esIPBlacklist": [],
		"kibanaIPWhitelist": [
			"0.0.0.0/0",
			"::/0"
		],
		"kibanaPrivateIPWhitelist": [],
		"publicIpWhitelist": [
			"::1",
			"0.0.0.0/0"
		],
		"kibanaDomain": "es-cn-n6w1ptcb30009****.kibana.elasticsearch.aliyuncs.com",
		"kibanaPort": 5601,
		"publicPort": 9200,
		"publicDomain": "es-cn-n6w1ptcb30009****.public.elasticsearch.aliyuncs.com",
		"haveKibana": true,
		"instanceCategory": "x-pack",
		"dedicateMaster": false,
		"advancedDedicateMaster": false,
		"masterConfiguration": {},
		"haveClientNode": false,
		"warmNode": false,
		"warmNodeConfiguration": {},
		"clientNodeConfiguration": {},
		"kibanaConfiguration": {
			"spec": "elasticsearch.n4.small",
			"amount": 1,
			"disk": 0
		},
		"dictList": [
			{
				"name": "SYSTEM_MAIN.dic",
				"fileSize": 3058510,
				"sourceType": "ORIGIN",
				"type": "MAIN"
			},
			{
				"name": "SYSTEM_STOPWORD.dic",
				"fileSize": 164,
				"sourceType": "ORIGIN",
				"type": "STOP"
			}
		],
		"synonymsDicts": [],
		"ikHotDicts": [],
		"aliwsDicts": [],
		"haveGrafana": false,
		"haveCerebro": false,
		"enableKibanaPublicNetwork": true,
		"enableKibanaPrivateNetwork": false,
		"advancedSetting": {
			"gcName": "CMS"
		}
	},
	"RequestId": "24A68E4C-94B4-45E4-9068-CB12F33C****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

