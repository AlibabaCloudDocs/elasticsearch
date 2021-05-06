# ListAvailableEsInstanceIds

调用ListAvailableEsInstanceIds，在设置Logstash实例的X-Pack监控时，获取可用的Elasticsearch实例列表（具备X-Pack监控能力）。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListAvailableEsInstanceIds&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/available-elasticsearch-for-centralized-management HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|description|String|instanceName|Elasticsearch实例名称。 |
|endpoint|String|http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200|Elasticsearch实例的公网访问地址。 |
|esInstanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|Elasticsearch实例ID。 |
|kibanaEndpoint|String|https://es-cn-n6w1o1x0w001c\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|Kibana的公网访问地址。 |

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/available-elasticsearch-for-centralized-management HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"esInstanceId": "es-cn-n6w1o1x0w001c****",
			"endpoint": "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200",
			"description": "pan_67_keepit",
			"kibanaEndpoint": "https://es-cn-n6w1o1x0w001c****.kibana.elasticsearch.aliyuncs.com:5601"
		},
		{
			"esInstanceId": "es-cn-6ja1rgego0006****",
			"endpoint": "http://es-cn-6ja1rgego0006****.elasticsearch.aliyuncs.com:9200",
			"description": "pan_mulitzones_keepit",
			"kibanaEndpoint": "https://es-cn-6ja1rgego0006****.kibana.elasticsearch.aliyuncs.com:5601"
		}
	],
	"RequestId": "4047A0E1-DEE0-4C6C-92A9-E52D32FD****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

