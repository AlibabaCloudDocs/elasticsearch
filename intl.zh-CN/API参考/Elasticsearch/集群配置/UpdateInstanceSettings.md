# UpdateInstanceSettings

调用UpdateInstanceSettings，更新指定实例的YML参数配置。

调用该接口时，请注意：

实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法更新配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateInstanceSettings&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/instance-settings HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入**esConfig**参数，用来指定待更新的YML参数和值，示例如下。

```

{
    "esConfig": {
        "thread_pool.bulk.queue_size": 500
    }
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BB1C321A-211C-4FD7-BD8B-7F2FABE2\*\*\*\*|请求ID。 |

返回数据中还包括Result参数，详情请参见[ListInstance](~~142230~~)。

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-nif1q9o8r0008****/instance-settings HTTP/1.1
公共请求头
{
    "esConfig": {
        "thread_pool.bulk.queue_size": 500
    }
}
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"instanceId": "es-cn-nif1q9o8r0008****",
		"version": "6.7.0_with_X-Pack",
		"description": "es-cn-nif1q9o8r0008****",
		"nodeAmount": 4,
		"paymentType": "postpaid",
		"status": "active",
		"privateNetworkIpWhiteList": [
			"0.0.0.0/0"
		],
		"enablePublic": false,
		"nodeSpec": {
			"spec": "elasticsearch.n4.small",
			"disk": 20,
			"diskType": "cloud_ssd",
			"diskEncryption": false
		},
		"networkConfig": {
			"vpcId": "vpc-bp16k1dvzxtmagcva****",
			"vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
			"vsArea": "cn-hangzhou-i",
			"type": "vpc"
		},
		"createdAt": "2020-07-07T04:05:16.791Z",
		"updatedAt": "2020-07-07T07:09:51.268Z",
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
			},
			{
				"configType": "aliVersion",
				"aliVersion": "ali1.2.0"
			}
		],
		"endTime": 4749811200000,
		"clusterTasks": [
			{
				"type": "MigrateData",
				"progress": 100,
				"detail": {},
				"status": "FINISHED",
				"canCancelable": false,
				"interruptible": false,
				"subTasks": [
					{
						"type": "FindShrinkNodeAction",
						"progress": 100,
						"detail": {},
						"status": "FINISHED",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					},
					{
						"type": "MigrateDataAction",
						"progress": 100,
						"detail": {
							"doneMigrateNodeIps": [
								"172.16.**.**"
							],
							"allMigrateNodeIps": [
								"172.16.**.**"
							]
						},
						"status": "FINISHED",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					}
				]
			}
		],
		"vpcInstanceId": "es-cn-nif1q9o8r0008****-worker",
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
		"domain": "es-cn-nif1q9o8r0008****.elasticsearch.aliyuncs.com",
		"port": 9200,
		"esVersion": "6.7.0_with_X-Pack",
		"esConfig": {
			"thread_pool.bulk.queue_size": "500",
			"xpack.security.authc.realms.native.type": "native",
			"xpack.security.authc.reserved_realm.enabled": "false",
			"xpack.security.transport.ssl.truststore.path": "168520994880****.p12",
			"xpack.security.authc.realms.native.order": "1",
			"xpack.license.self_generated.type": "trial",
			"xpack.security.authc.realms.file.order": "0",
			"xpack.security.authc.realms.file.type": "file",
			"xpack.security.enabled": "true",
			"bootstrap.memory_lock": "true",
			"xpack.monitoring.collection.enabled": "true",
			"xpack.security.transport.ssl.keystore.path": "168520994880****.p12",
			"xpack.security.transport.ssl.verification_mode": "certificate",
			"xpack.security.transport.ssl.enabled": "true"
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
		"publicIpWhitelist": [],
		"kibanaDomain": "es-cn-nif1q9o8r0008****.kibana.elasticsearch.aliyuncs.com",
		"kibanaPort": 5601,
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
				"fileSize": 2782602,
				"sourceType": "ORIGIN",
				"type": "MAIN"
			},
			{
				"name": "SYSTEM_STOPWORD.dic",
				"fileSize": 132,
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
	"RequestId": "C1FA70F8-B84E-4D89-B31A-BCD1E476****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

