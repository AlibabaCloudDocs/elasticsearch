# UpdateLogstashDescription

调用UpdateLogstashDescription，修改指定Logstash实例的名称。

调用该接口时，请注意：

实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法修改实例名称。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateLogstashDescription&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/logstashes/[InstanceId]/description HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-n6w1o5jq\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入description字段，用来指定修改后的实例名称，示例如下。

```

{
    "description": "logstash_name"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|description|String|logstash\_name|实例名称。 |

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

|ls-cn-abc

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

|实例的状态，支持四种状态：正常（active）、生效中（activating）、冻结（inactive）和失效（invalid）。 |
|esVersion

|String

|6.7.0\_with\_X-Pack

|实例的版本。 |
|createdAt

|String

|2018-07-13T03:58:07.253Z

|实例的创建时间。 |
|updatedAt

|String

|2018-07-13T03:58:07.253Z

|实例最后更新的时间。 |
|nodeSpec

| | |节点的配置信息。 |
|└spec

|String

|logstash.n4.small

|节点规格。 |
|└disk

|Integer

|40

|节点的硬盘大小。 |
|networkConfig

| | |网络配置。 |
|└type

|String

|vpc

|网络类型。目前只支持专有网络VPC（Virtual Private Cloud）。 |
|└vpcId

|String

|vpc-abc

|VPC ID。 |
|└vswitchId

|String

|vsw-abc

|虚拟交换机ID。 |
|vsArea

|String

|cn-hangzhou-\*

|实例所在的可用区。 |
|domainList

| | |域名列表。 |
|  └domain

|String

|ls-cn-abc.logstash.aliyuncs.com

|实例分配的私网域名。 |
|  └port

|Integer

|7001

|端口号。 |

**说明：** └表示子参数，其他参数说明请参见[ListLogstash](~~160534~~)。

## 示例

请求示例

```
PATCH /openapi/logstashes/ls-cn-n6w1o5jq****/description HTTP/1.1
公共请求头
{
    "description": "logstash_name"
}
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"instanceId": "ls-cn-n6w1o5jq****",
		"version": "6.7.0_with_X-Pack",
		"description": "logstash_name",
		"nodeAmount": 1,
		"paymentType": "postpaid",
		"status": "active",
		"enablePublic": false,
		"nodeSpec": {
			"spec": "elasticsearch.sn1ne.large",
			"disk": 50,
			"diskType": "cloud_ssd"
		},
		"networkConfig": {
			"vpcId": "vpc-bp16k1dvzxtmagcva****",
			"vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
			"vsArea": "cn-hangzhou-i",
			"type": "vpc"
		},
		"createdAt": "2020-05-27T01:30:15.947Z",
		"updatedAt": "2020-07-08T02:38:47.137Z",
		"commodityCode": "elasticsearch_logstash_post",
		"extendConfigs": [],
		"endTime": 4749897600000,
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
	"RequestId": "C3845099-3D0E-4D2B-9D62-F16019EE****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

