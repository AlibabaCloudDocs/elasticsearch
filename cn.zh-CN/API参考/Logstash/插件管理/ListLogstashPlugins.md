# ListLogstashPlugins

调用ListLogstashPlugins，获取所有或指定插件的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListLogstashPlugins&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/plugins HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|ls-cn-oew1qbgl\*\*\*\*|实例ID。 |
|name|String|否|logstash-filter-clone|插件名称。 |
|page|Integer|否|10|插件列表的分页数。起始值：1，默认值：1。 |
|size|Integer|否|3|分页查询时设置的每页条数。最大值：100，默认值：10。 |
|source|String|否|USER|插件来源。可选值：

 -   USER：自定义插件
-   SYSTEM：系统预置插件 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|description|String|The clone filter is for duplicating events.|插件描述。 |
|name|String|logstash-filter-clone|插件名称。 |
|source|String|SYSTEM|插件来源。 |
|specificationUrl|String|https://xxx.html|插件的说明文档地址。 |
|state|String|INSTALLED|插件的状态。 |

返回数据中还包含以下参数。

|名称

|类型

|示例值

|描述 |
|----|----|-----|----|
|Headers

|Struct

| |返回头信息。 |
|└X-Total-Count

|Integer

|131

|返回的插件数量。 |

**说明：** └表示子参数。

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/plugins?name=logstash-filter-clone&page=10&size=3 HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <name>logstash-filter-clone</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>The clone filter is for duplicating events. A clone will be created for each type in the clone list.</description>
    <specificationUrl>https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-clone.html</specificationUrl>
</Result>
<Result>
    <name>logstash-filter-csv</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>The CSV filter takes an event field containing CSV data, parses it, and stores it as individual fields (can optionally specify the names). This filter can also parse data with any separator, not just commas.</description>
    <specificationUrl>https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-csv.html</specificationUrl>
</Result>
<Result>
    <name>logstash-filter-date</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>The date filter is used for parsing dates from fields, and then using that date or timestamp as the logstash timestamp for the event.</description>
    <specificationUrl>https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-date.html</specificationUrl>
</Result>
<RequestId>40C0570B-AB40-48BA-8CAE-66EA230A****</RequestId>
<Headers>
    <X-Total-Count>131</X-Total-Count>
</Headers>
```

`JSON` 格式

```
{
	"Result": [
		{
			"name": "logstash-filter-clone",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "The clone filter is for duplicating events. A clone will be created for each type in the clone list.",
			"specificationUrl": "https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-clone.html"
		},
		{
			"name": "logstash-filter-csv",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "The CSV filter takes an event field containing CSV data, parses it, and stores it as individual fields (can optionally specify the names). This filter can also parse data with any separator, not just commas.",
			"specificationUrl": "https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-csv.html"
		},
		{
			"name": "logstash-filter-date",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "The date filter is used for parsing dates from fields, and then using that date or timestamp as the logstash timestamp for the event.",
			"specificationUrl": "https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-date.html"
		}
	],
	"RequestId": "40C0570B-AB40-48BA-8CAE-66EA230A****",
	"Headers": {
		"X-Total-Count": 131
	}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

