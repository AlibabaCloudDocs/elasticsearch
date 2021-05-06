# UpdateLogstash

调用UpdateLogstash，修改指定实例的部分信息，例如节点数、配额、名称、硬盘大小等。

调用该接口时，请注意：

实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法修改实例信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateLogstash&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/logstashes/[InstanceId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-n6w1o5jq\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需指定要修改的配置，示例如下。

```

{
  "nodeSpec": {
      "disk": 40
    }
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果。 |

Result中还包含以下参数。

|参数名称

|数据类型

|示例值

|描述 |
|------|------|-----|----|
|instanceId

|String

|ls-cn-n6w1o5jq\*\*\*\*

|实例ID。 |
|description

|String

|test

|实例名称。 |
|nodeAmount

|Integer

|2

|实例的节点个数。 |
|paymentType

|String

|postpaid

|实例的付费方式。支持：prepaid（包年包月）、postpaid（按量付费）。 |
|status

|String

|active

|实例的状态，支持：正常（active）、生效中（activating）、冻结（inactive）和失效（invalid）。 |
|version

|String

|6.7.0\_with\_X-Pack

|实例版本。 |
|createdAt

|String

|2018-07-13T03:58:07.253Z

|实例的创建时间。 |
|updatedAt

|String

|2018-07-18T10:10:04.484Z

|实例最后更新的时间。 |
|nodeSpec

| | |节点的配置信息。 |
|└spec

|String

|logstash.sn2ne.xlarge

|节点的规格。 |
|└disk

|Integer

|40

|节点的硬盘大小。 |
|networkConfig

| | |网络配置。 |
|└type

|String

|vpc

|网络类型，目前只支持专有网络VPC（Virtual Private Cloud）。 |
|└vpcId

|String

|vpc-bp16k1dvzxtmagcva\*\*\*\*

|专有网络ID。 |
|└vswitchId

|String

|vsw-bp1k4ec6s7sjdbudw\*\*\*\*

|交换机ID。 |
|vsArea

|String

|cn-hangzhou-a

|实例所在的可用区。 |

**说明：** └表示子参数，更多参数说明请参见[ListLogstash](~~160534~~)。

## 示例

请求示例

```
PATCH /openapi/logstashes/ls-cn-n6w1o5jq**** HTTP/1.1
公共请求头
{
  "nodeSpec": {
      "disk": 40
    }
}
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"instanceId": "ls-cn-n6w1o5jq****",
		"version": "6.7.0_with_X-Pack",
		"description": "test",
		"nodeAmount": 1,
		"paymentType": "postpaid",
		"status": "active",
		"enablePublic": false,
		"nodeSpec": {
			"spec": "elasticsearch.sn1ne.large",
			"disk": 40,
			"diskType": "cloud_ssd"
		},
		"networkConfig": {
			"vpcId": "vpc-bp16k1dvzxtmagcva****",
			"vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
			"vsArea": "cn-hangzhou-i",
			"type": "vpc"
		},
		"createdAt": "2020-05-27T01:30:15.947Z",
		"updatedAt": "2020-05-27T01:40:51.333Z",
		"commodityCode": "elasticsearch_logstash_post",
		"extendConfigs": [],
		"endTime": 4746268800000,
		"clusterTasks": [],
		"resourceGroupId": "rg-acfm2h5vbzd****",
		"zoneCount": 1,
		"protocol": "HTTP",
		"zoneInfos": [
			{
				"zoneId": "cn-hangzhou-i",
				"status": "NORMAL"
			}
		],
		"instanceType": "logstash",
		"inited": true,
		"tags": [],
		"config": {
			"xpack.monitoring.elasticsearch.username": "elastic",
			"xpack.monitoring.enabled": "true",
			"slowlog.threshold.debug": "500ms",
			"xpack.monitoring.elasticsearch.password": "Elasti****",
			"xpack.monitoring.elasticsearch.hosts": "[\"http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200\"]",
			"slowlog.threshold.warn": "2s",
			"slowlog.threshold.info": "1s",
			"slowlog.threshold.trace": "100ms"
		},
		"endpointList": [
			{
				"host": "172.16.**.**",
				"port": 9600,
				"zoneId": "cn-hangzhou-i"
			}
		]
	},
	"RequestId": "FBD56B2B-367F-470D-90D0-C3120832****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

