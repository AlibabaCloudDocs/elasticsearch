# DescribePipelineManagementConfig

调用DescribePipelineManagementConfig，获取Logstash管道管理配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribePipelineManagementConfig&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/pipeline-management-config HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|endpoints|String|\["http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|Elasticsearch实例的访问地址列表，格式为：`域名:端口号`。 |
|esInstanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|Elasticsearch实例ID。 |
|pipelineIds|List|\["testKibanaManagement"\]|管道名称列表。 |
|pipelineManagementType|String|MULTIPLE\_PIPELINE|管道管理方式。支持Kibana和MULTIPLE\_PIPELINE。 |
|userName|String|elastic|访问实例的用户名。 |

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/pipeline-management-config HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"pipelineManagementType": "MULTIPLE_PIPELINE",
		"esInstanceId": "es-cn-n6w1o1x0w001c****",
		"endpoints": [
			"http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
		],
		"pipelineIds": [
			"testKibanaManagement"
		],
		"userName": "elastic"
	},
	"RequestId": "6822F07C-A896-4A2C-A430-BC01D5D1****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

