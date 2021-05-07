# ListKibanaPlugins

调用ListKibanaPlugins，获取Kibana插件列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListKibanaPlugins&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/kibana-plugins HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-oew1q8bev0002\*\*\*\*|实例ID。 |
|page|String|Query|否|1|实例列表的页码。默认值：1。 |
|size|Integer|Query|否|10|分页查询时设置的每页条数。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |请求头。 |
|X-Total-Count|Integer|3|返回的数据条数。 |
|RequestId|String|11234B4A-34CE-473B-8E61-AD95702E\*\*\*\*|请求ID。 |
|Result|Array of PluginItem| |当前请求返回的插件信息。 |
|description|String|Customize DSL statements to query data.|插件描述。 |
|name|String|bsearch\_querybuilder|插件名称。 |
|source|String|SYSTEM|插件来源。 |
|specificationUrl|String|https://xxxx|插件简介地址，支持null。 |
|state|String|INSTALLED|插件安装状态。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-oew1q8bev0002****/kibana-plugins?page=1&size=10 HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"name": "bsearch_label",
			"state": "UNINSTALLED",
			"source": "SYSTEM",
			"description": "Mark data in a visual way to complete the query tasks quickly and easily.",
			"specificationUrl": "https://xxx.html"
		},
		{
			"name": "bsearch_querybuilder",
			"state": "UNINSTALLED",
			"source": "SYSTEM",
			"description": "Customize DSL statements to query data.",
			"specificationUrl": "https://xxx.html"
		},
		{
			"name": "network_vis",
			"state": "UNINSTALLED",
			"source": "SYSTEM",
			"description": "This is a plugin developed for Kibana that displays a network node that link two fields that have been previously selected."
		}
	],
	"RequestId": "11234B4A-34CE-473B-8E61-AD95702E****",
	"Headers": {
		"X-Total-Count": 3
	}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

