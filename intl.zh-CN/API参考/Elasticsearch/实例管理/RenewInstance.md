# RenewInstance

调用RenewInstance，为包年包月实例续费。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=RenewInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/renew HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B350\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还要填入以下字段，用来指定续费信息。

|参数

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|duration

|Integer

|是

|1

|续费时长。如果pricingCycle为Year，可选时长为1~3；如果pricingCycle为Month，可选时长为1~9。 |
|pricingCycle

|String

|是

|Year

|续费周期。可选值：Year（按年续费）、Month（按月续费）。 |

示例如下。

```

{
    "duration":1,
    "pricingCycle":"Year"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：续费成功
-   false：续费失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/renew HTTP/1.1
公共请求头
{
    "duration":1,
    "pricingCycle":"Year"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>4FF74B95-7D01-44B4-8E0D-6E5AB515****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "4FF74B95-7D01-44B4-8E0D-6E5AB515****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

