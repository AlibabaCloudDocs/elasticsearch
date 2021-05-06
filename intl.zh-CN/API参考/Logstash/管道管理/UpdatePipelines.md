# UpdatePipelines

调用UpdatePipelines，更新Logstash管道信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdatePipelines&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/logstashes/[InstanceId]/pipelines HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|trigger|Boolean|Query|否|false|是否立即部署。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定更新的管道信息。

**说明：** RequestBody中需填入JSON格式的pipeline数组。

|名称

|类型

|是否必选

|示例值

|描述

| |
|----|----|------|-----|----|--|
|pipelineId

|String

|是

|pipeline-test

|待更新的管道的ID。

| |
|description

|String

|否

|this is a test

|管道描述。 |
|config

|String

|是

|input \{ \} filter \{ \} output \{ \}

|管道具体配置。 |
|workers

|Integer

|否

|2

|管道工作线程数。

| |
|batchSize

|Integer

|否

|125

|管道批大小。 |
|batchDelay

|Integer

|否

|50

|管道批延迟。 |
|queueType

|String

|否

|MEMORY

|队列类型，支持：MEMORY、PERSISTED。 |
|pipelineStatus

|String

|否

|RUNNING

|管道状态，支持：NOT\_DEPLOYED、RUNNING、DELETED。trigger为true时，必填。

| |
|queueMaxBytes

|Integer

|否

|1024

|队列最大字节数。 |
|queueCheckPointWrites

|Integer

|否

|1024

|队列检查点写入数。 |

示例如下。

```

[
    {
        "pipelineId": "test",
        "config": "input {\n\n}\nfilter {\n\n}\noutput {\n \n}"
    }
]

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：更新成功
-   false：更新失败 |

## 示例

请求示例

```
PUT /openapi/logstashes/ls-cn-oew1qbgl****/pipelines?clientToken=5A2CFF0E-5718-45B5-9D4D-70B3FF****&trigger=false HTTP/1.1
公共请求头
[
{"pipelineId":"test", "config":"input {\n\n}\nfilter {\n\n}\noutput {\n \n}"},

{"pipelineId":"test1", "config":"input {\n\n}\nfilter {\n\n}\noutput {\n  \n}"}
]
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "734F9DEC-74AC-4D6E-B97F-FFDAA90B****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

