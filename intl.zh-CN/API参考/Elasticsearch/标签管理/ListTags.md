# ListTags

调用ListTags，查询所有可见的用户标签。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListTags&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/tags/all-tags HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|pageSize|Integer|Query|否|20|返回结果的分页数。默认值：50。 |
|resourceType|String|Query|否|INSTANCE|资源类型，固定为INSTANCE。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|TagKey|String|env|标签键。 |
|TagValue|String|dev|标签值。 |

## 示例

请求示例

```
GET /openapi/tags/all-tags?pageSize=20 HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"TagKey": "aa",
			"TagValue": "ddd"
		},
		{
			"TagKey": "manager",
			"TagValue": "all_persion"
		},
		{
			"TagKey": "dev",
			"TagValue": "leizhang"
		},
		{
			"TagKey": "a",
			"TagValue": "b"
		},
		{
			"TagKey": "zl",
			"TagValue": "keept"
		},
		{
			"TagKey": "zl",
			"TagValue": ""
		},
		{
			"TagKey": "c",
			"TagValue": "d"
		},
		{
			"TagKey": "tt",
			"TagValue": "tt"
		},
		{
			"TagKey": "acs:rm:rgId",
			"TagValue": "rg-acfm2h5vbzd****"
		},
		{
			"TagKey": "acs:rm:rgId",
			"TagValue": "rg-aek22gedcbf****"
		},
		{
			"TagKey": "estag",
			"TagValue": "instance"
		}
	],
	"RequestId": "5ADCBB89-6596-4AF3-94A5-64E5393A****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

