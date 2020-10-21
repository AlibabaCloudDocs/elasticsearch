# StopCollector

调用StopCollector，停止运行中的采集器。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=StopCollector&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/collectors/[ResId]/actions/stop HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ResId|String|是|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器ID。 |
|ClientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果。 |

## 示例

请求示例

```
POST /openapi/collectors/ct-cn-77uqof2s7rg5c****/actions/stop HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>7A29449F-A241-4A5D-92B5-B1ABA4F5****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "7A29449F-A241-4A5D-92B5-B1ABA4F5****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

