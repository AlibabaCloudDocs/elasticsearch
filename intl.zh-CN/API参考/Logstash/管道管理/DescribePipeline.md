# DescribePipeline

调用DescribePipeline，获取Logstash实例的管道信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribePipeline&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/pipelines/[PipelineId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|PipelineId|String|Path|是|pipeline\_test|管道ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|829F38F6-E2D6-4109-90A6-888160BD1\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|batchDelay|Integer|50|管道批延迟。 |
|batchSize|Integer|125|管道批大小。 |
|config|String|input \{ \} filter \{ \} output \{ \}|管道具体配置。 |
|description|String|this is a test|管道描述。 |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|管道创建时间。 |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|管道更新时间。 |
|pipelineId|String|pipeline\_test|管道ID。 |
|pipelineStatus|String|RUNNING|管道状态。支持：NOT\_DEPLOYED、RUNNING和DELETED。 |
|queueCheckPointWrites|Integer|1024|队列检查点写入数。 |
|queueMaxBytes|Integer|1024|队列最大字节数。 |
|queueType|String|memory|队列类型。支持：MEMORY和PERSISTED。 |
|workers|Integer|2|管道工作线程数。 |

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/pipelines/pipeline_test HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"pipelineId": "test",
		"config": "input {\n\n}\nfilter {\n\n}\noutput {\n\n}",
		"workers": 2,
		"batchSize": 100,
		"batchDelay": 60,
		"queueType": "MEMORY",
		"pipelineStatus": "NOT_DEPLOYED",
		"queueMaxBytes": 1024,
		"queueCheckPointWrites": 1024,
		"gmtCreatedTime": "2020-09-16T06:35:30.139Z",
		"gmtUpdateTime": "2020-09-16T07:06:24.759Z"
	},
	"RequestId": "49AEA9F3-B946-4102-8BD7-92E71C86****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

