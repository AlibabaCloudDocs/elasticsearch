# ListInstanceIndices

调用ListInstanceIndices，获取集群的索引列表。能够过滤系统索引。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListInstanceIndices&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/diagnosis/instances/[InstanceId]/indices HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|lang|String|否|en|配置语言，支持多种语言。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|List|\["index1","index2","index3"\]|返回结果，索引列表。 |

## 示例

请求示例

```
GET /openapi/diagnosis/instances/es-cn-n6w1o1x0w001c****/indices HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>filebeat-6.7.0-2020.09.12</Result>
<Result>filebeat-6.7.0-2020.07.30</Result>
<Result>test</Result>
<Result>filebeat-6.7.0-2020.09.11</Result>
<Result>filebeat-6.7.0-2020.08.16</Result>
<Result>my_index_aliws</Result>
<RequestId>5BF8C741-26F8-4F67-B399-E49E1A05****</RequestId>
```

`JSON` 格式

```
{
	"Result": [
		"filebeat-6.7.0-2020.09.12",
		"filebeat-6.7.0-2020.07.30",
		"test",
		"filebeat-6.7.0-2020.09.11",
		"filebeat-6.7.0-2020.08.16",
		"my_index_aliws"
	],
	"RequestId": "5BF8C741-26F8-4F67-B399-E49E1A05****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

