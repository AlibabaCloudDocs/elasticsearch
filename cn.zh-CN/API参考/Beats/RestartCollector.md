# RestartCollector

调用RestartCollector，重启采集器。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=RestartCollector&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/collectors/[ResId]/actions/restart HTTPS|HTTP
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ResId|String|Path|是|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|84B4038A-AF38-4BF4-9FAD-EA92A4FFF00A|请求ID。 |
|Result|Boolean|true|返回结果，支持：

 -   true：重启成功
-   false：重启失败 |

## 示例

请求示例

```
POST /openapi/collectors/ct-cn-tfv81t7vs8608****/actions/restart HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>true</Result>
<RequestId>84B4038A-AF38-4BF4-9FAD-EA92A4FFF00A</RequestId>
```

`JSON`格式

```
{
	"Result": true,
	"RequestId": "84B4038A-AF38-4BF4-9FAD-EA92A4FFF00A"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

