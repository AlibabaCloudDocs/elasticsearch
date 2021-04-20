# DeleteILMPolicy

调用DeleteILMPolicy，删除指定的生命周期策略定义。

**说明：** 您无法删除当前正在使用的策略。如果该策略正用于管理任一索引，则请求将失败并返回错误。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DeleteILMPolicy&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/instances/[InstanceId]/ilm-policies/[PolicyName] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif24adwc0082\*\*\*\*|实例ID。 |
|PolicyName|String|Path|是|slm-history-ilm-policy|索引生命周期策略名称。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|694FDC20-0FDD-47C4-B921-BFF902FA\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：成功删除指定的生命周期策略定义。
-   false：删除指定的生命周期策略定义失败。 |

## 示例

请求示例

```
DELETE /openapi/instances/es-cn-nif24adwc0082****/ilm-policies/slm-history-ilm-policy HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<RequestId>694FDC20-0FDD-47C4-B921-BFF902FA****</RequestId>
<Result>true</Result>
```

`JSON`格式

```
{
    "RequestId": "694FDC20-0FDD-47C4-B921-BFF902FA****",
    "Result": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

