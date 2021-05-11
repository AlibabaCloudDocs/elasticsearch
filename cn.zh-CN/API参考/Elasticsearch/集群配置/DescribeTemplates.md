# DescribeTemplates

调用DescribeTemplates，获取实例的场景模板配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeTemplates&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/templates HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|content|String|\{\\n\\t\\"persistent\\":\{\\n\\t\\t\\"search\\":\{\\n\\t\\t\\t\\"max\_buckets\\":\\"10000\\"\\n\\t\\t\}\\n\\t\}\\n\}|模板内容。 |
|templateName|String|dynamicSettings|模板名称。支持：

 -   staticSettings：elasticsearch.yml配置
-   ilmPolicy：索引生命周期配置
-   indexTemplate：索引模板配置
-   dynamicSettings：集群动态配置 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/templates HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"content": "",
			"templateName": "staticSettings"
		},
		{
			"content": "",
			"templateName": "ilmPolicy"
		},
		{
			"content": "{\n\t\"order\":-2147483647,\n\t\"index_patterns\":[\n\t\t\"*\"\n\t],\n\t\"settings\":{\n\t\t\"index\":{\n\t\t\t\"search\":{\n\t\t\t\t\"slowlog\":{\n\t\t\t\t\t\"level\":\"info\",\n\t\t\t\t\t\"threshold\":{\n\t\t\t\t\t\t\"fetch\":{\n\t\t\t\t\t\t\t\"warn\":\"200ms\",\n\t\t\t\t\t\t\t\"trace\":\"50ms\",\n\t\t\t\t\t\t\t\"debug\":\"80ms\",\n\t\t\t\t\t\t\t\"info\":\"100ms\"\n\t\t\t\t\t\t},\n\t\t\t\t\t\t\"query\":{\n\t\t\t\t\t\t\t\"warn\":\"500ms\",\n\t\t\t\t\t\t\t\"trace\":\"50ms\",\n\t\t\t\t\t\t\t\"debug\":\"100ms\",\n\t\t\t\t\t\t\t\"info\":\"200ms\"\n\t\t\t\t\t\t}\n\t\t\t\t\t}\n\t\t\t\t}\n\t\t\t},\n\t\t\t\"refresh_interval\":\"10s\",\n\t\t\t\"unassigned\":{\n\t\t\t\t\"node_left\":{\n\t\t\t\t\t\"delayed_timeout\":\"5m\"\n\t\t\t\t}\n\t\t\t},\n\t\t\t\"indexing\":{\n\t\t\t\t\"slowlog\":{\n\t\t\t\t\t\"level\":\"info\",\n\t\t\t\t\t\"threshold\":{\n\t\t\t\t\t\t\"index\":{\n\t\t\t\t\t\t\t\"warn\":\"200ms\",\n\t\t\t\t\t\t\t\"trace\":\"20ms\",\n\t\t\t\t\t\t\t\"debug\":\"50ms\",\n\t\t\t\t\t\t\t\"info\":\"100ms\"\n\t\t\t\t\t\t}\n\t\t\t\t\t},\n\t\t\t\t\t\"source\":\"1000\"\n\t\t\t\t}\n\t\t\t},\n\t\t\t\"number_of_shards\":\"1\"\n\t\t}\n\t}\n}",
			"templateName": "indexTemplate"
		},
		{
			"content": "{\n\t\"persistent\":{\n\t\t\"search\":{\n\t\t\t\"max_buckets\":\"10000\"\n\t\t}\n\t}\n}",
			"templateName": "dynamicSettings"
		}
	],
	"RequestId": "E5C65630-AB52-4DBA-9269-DECCE5E1****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

