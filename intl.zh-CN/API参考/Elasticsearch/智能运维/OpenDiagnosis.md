# OpenDiagnosis

调用OpenDiagnosis，打开实例的智能运维功能。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=OpenDiagnosis&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/diagnosis/instances/[InstanceId]/actions/open-diagnosis HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|lang|String|Query|否|en|语言配置，支持多种语言。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：开启智能运维功能成功
-   false：开启智能运维功能失败 |

## 示例

请求示例

```
POST /openapi/diagnosis/instances/es-cn-n6w1o1x0w001c****/actions/open-diagnosis HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "37F0EC9E-B81A-43D9-B583-9D5A8DEC****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

