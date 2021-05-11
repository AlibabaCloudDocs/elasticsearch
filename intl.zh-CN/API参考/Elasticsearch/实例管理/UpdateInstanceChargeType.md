# UpdateInstanceChargeType

调用UpdateInstanceChargeType，将按量付费实例转换为包年包月实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateInstanceChargeType&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/convert-pay-type HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-0pp1jxvcl000z\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定转换为包年包月后，实例的付费信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|paymentInfo

|Array

|是

| |实例转换后的付费信息。 |
|└duration

|Integer

|是

|1

|付费时长。如果pricingCycle为Year，可选值：1~3；如果pricingCycle为Month，可选值：1~9。 |
|└pricingCycle

|String

|是

|Year

|付费周期。可选值：Year、Month。 |
|paymentType

|String

|是

|prepaid

|实例当前的付费类型。目前只支持将按量付费实例转换为包年包月，因此该参数值固定为prepaid。 |

**说明：** └表示子参数。

示例如下。

```

{
  "paymentInfo":{ 
       "duration":1,
       "pricingCycle":"Month"
    },
  "paymentType":"prepaid"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：转换成功
-   false：转换失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-0pp1jxvcl000z****/actions/convert-pay-type HTTP/1.1
公共请求头
{
  "paymentInfo":{ 
       "duration":1,
       "pricingCycle":"Month"
    },
  "paymentType":"prepaid"
}
```

正常返回示例

`JSON`格式

```
{
    "Result":true,
    "RequestId":"3760F67B-691D-4663-B4E5-6783554F****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

