# RecommendTemplates

调用RecommendTemplates，获取推荐的集群配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=RecommendTemplates&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/recommended-templates HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-oew20apwz0007\*\*\*\*|集群ID。 |
|usageScenario|String|Query|是|general|集群使用的场景化模板类型。可选值：

 -   general：通用场景
-   analysisVisualization：数据分析场景
-   dbAcceleration：数据库加速场景
-   search：搜索场景
-   log：日志场景

 **说明：** 商业版实例支持通用场景、数据分析场景、数据库加速场景和搜索场景；日志增强版仅支持日志场景。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|66B060CF-7381-49C7-9B89-7757927FDA16|请求ID。 |
|Result|Array of TemplateConfig| |返回结果。 |
|content|String|\{\\n\\t\\"persistent\\": \{\\n\\t\\t\\"search\\": \{\\n\\t\\t\\t\\"max\_buckets\\": \\"10000\\"\\n\\t\\t\}\\n\\t\}\\n\}|模板配置内容。 |
|templateName|String|dynamicSettings|模板名称。支持：

 -   staticSettings：集群静态配置
-   dynamicSettings：集群动态配置
-   indexTemplate：索引模板配置
-   ilmPolicy：索引生命周期配置

 **说明：** 6.7.0及以上版本的日志增强版实例，支持启用索引生命周期模板。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-oew20apwz0007****/recommended-templates?usageScenario=general HTTP/1.1
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
			"content": "{\n\t\"order\": -2147483647,\n\t\"index_patterns\": [\n\t\t\"*\"\n\t],\n\t\"settings\": {...}.....}",
			"templateName": "indexTemplate"
		},
		{
			"content": "{\n\t\"persistent\": {\n\t\t\"search\": {\n\t\t\t\"max_buckets\": \"10000\"\n\t\t}\n\t}\n}",
			"templateName": "dynamicSettings"
		}
	],
	"RequestId": "66B060CF-7381-49C7-9B89-7757927FDA16"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

