# DescribeRegions

调用DescribeRegions，获取阿里云Elasticsearch的区域信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeRegions&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/regions HTTP/1.1
```

## 请求参数

无

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of regionInfo| |返回结果列表。 |
|consoleEndpoint|String|https://elasticsearch-cn-hangzhou.console.aliyun.com|该区域控制台暴露的Endpoint地址。 |
|localName|String|China \(Hangzhou\)|区域名称。 |
|regionEndpoint|String|elasticsearch.cn-hangzhou.aliyuncs.com|该区域的Endpoint地址。 |
|regionId|String|cn-hangzhou|区域ID。 |
|status|String|available|该区域的可用状态。 |

## 示例

请求示例

```
GET /openapi/regions?RegionId=cn-hangzhou HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		  {
			"regionId": "cn-hangzhou",
			"localName": "China (Hangzhou)",
			"regionEndpoint": "elasticsearch.cn-hangzhou.aliyuncs.com",
			"consoleEndpoint": "https://elasticsearch-cn-hangzhou.console.aliyun.com",
			"status": "available"
		  }
        ]
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

