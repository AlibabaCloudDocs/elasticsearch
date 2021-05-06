# ListPipeline

调用ListPipeline，获取Logstash实例的管道列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListPipeline&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/pipelines HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|pipelineId|String|Query|否|pipeline\_test|管道ID。 |
|page|Integer|Query|否|1|返回结果的分页数。默认值：1，最小值：1，最大值：200。 |
|size|Integer|Query|否|15|每页的管道数。最小值：1，最大值：200。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|2|总记录数。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|gmtCreatedTime|String|2020-08-05T03:10:38.188Z|管道创建时间。 |
|gmtUpdateTime|String|2020-08-05T08:43:31.757Z|管道更新时间。 |
|pipelineId|String|pipeline\_test|管道ID。 |
|pipelineStatus|String|NOT\_DEPLOYED|管道状态，支持：NOT\_DEPLOYED、RUNNING和DELETED。 |

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/pipelines?pipelineId=test HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"pipelineId": "datahub_test",
			"pipelineStatus": "RUNNING",
			"gmtCreatedTime": "2020-09-09T02:21:28.844Z",
			"gmtUpdateTime": "2020-09-09T06:09:43.796Z"
		},
		{
			"pipelineId": "test",
			"pipelineStatus": "NOT_DEPLOYED",
			"gmtCreatedTime": "2020-09-16T06:35:30.139Z",
			"gmtUpdateTime": "2020-09-16T07:06:24.759Z"
		},
		{
			"pipelineId": "test_1",
			"pipelineStatus": "NOT_DEPLOYED",
			"gmtCreatedTime": "2020-09-16T06:33:05.290Z",
			"gmtUpdateTime": "2020-09-16T06:33:05.290Z"
		}
	],
	"RequestId": "AA6771E2-4007-4F1F-ADCB-16DAABB9****",
	"Headers": {
		"X-Total-Count": 3
	}
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

