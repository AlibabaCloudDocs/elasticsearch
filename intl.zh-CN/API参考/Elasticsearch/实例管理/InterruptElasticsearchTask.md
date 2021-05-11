# InterruptElasticsearchTask

调用InterruptElasticsearchTask，中断变更中的阿里云Elasticsearch实例。仅对状态为生效中的实例有效，中断后，实例进入变更中断（suspended）状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=InterruptElasticsearchTask&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/interrupt HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|InstanceStatusNotSupportCurrentAction|错误码。仅当返回异常时显示。 |
|Message|String|The cluster is running tasks or in an error status. Try again later.|错误信息。仅当返回异常时显示。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：中断变更成功
-   false：中断变更失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/interrupt HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "E9908D15-13F4-4428-B08F-A3EE8E39****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

