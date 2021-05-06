# RestartLogstash

调用RestartLogstash，重启指定实例。重启后，实例会进入生效中（activing）状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=RestartLogstash&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/actions/restart HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-v0h1kzca\*\*\*\*|实例ID。 |
|force|Boolean|Query|否|true|是否强制重启。true表示强制，false表示不强制。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |

返回数据中还包括Result参数，参数说明请参见[ListLogstash](~~160534~~)。

## 示例

请求示例

```
POST /openapi/logstashes/ls-cn-v0h1kzca****/actions/restart HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"instanceId": "ls-cn-v0h1kzca****",
		"version": "7.4.0_with_X-Pack",
		"description": "es-74-keepit",
		"nodeAmount": 1,
		"paymentType": "prepaid",
		"status": "active",
		"enablePublic": false,
		"nodeSpec": {
			"spec": "elasticsearch.sn1ne.large",
			"disk": 20,
			"diskType": "cloud_ssd"
		},
		"networkConfig": {
			"vpcId": "vpc-bp12nu14urf0upaf4****",
			"vswitchId": "vsw-bp131d5ag0vjd5ja3****",
			"vsArea": "cn-hangzhou-h",
			"type": "vpc"
		},
		"createdAt": "2020-03-26T09:23:06.575Z",
		"updatedAt": "2020-05-12T11:06:14.132Z",
		"commodityCode": "elasticsearch_logstash_pre",
		"extendConfigs": [],
		"endTime": 1619884800000,
		"clusterTasks": [],
		"resourceGroupId": "rg-acfm2h5vbzd****",
		"zoneCount": 1,
		"protocol": "HTTP",
		"zoneInfos": [
			{
				"zoneId": "cn-hangzhou-h",
				"status": "NORMAL"
			}
		],
		"instanceType": "logstash",
		"inited": true,
		"tags": [],
		"config": {
			"slowlog.threshold.warn": "2s",
			"slowlog.threshold.info": "1s",
			"slowlog.threshold.debug": "500ms",
			"slowlog.threshold.trace": "100ms"
		},
		"endpointList": [
			{
				"host": "10.7.**.**",
				"port": 9600,
				"zoneId": "cn-hangzhou-h"
			}
		]
	},
	"RequestId": "831AD23B-175F-47F1-8314-AFBB9947****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

