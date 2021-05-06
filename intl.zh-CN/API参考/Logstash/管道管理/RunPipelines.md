# RunPipelines

调用RunPipelines，立即部署Logstash管道。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=RunPipelines&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/pipelines/action/run HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入管道ID列表，用来指定待部署的管道，示例：`["PipelineId1","PipelineId2","..."]`。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：部署成功
-   false：部署失败 |

## 示例

请求示例

```
POST /openapi/logstashes/ls-cn-oew1qbgl****/pipelines/action/run HTTP/1.1
公共请求头
["PipelineId1","PipelineId2","..."]
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "529AEFD2-F5FA-492F-8921-520141B9****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

