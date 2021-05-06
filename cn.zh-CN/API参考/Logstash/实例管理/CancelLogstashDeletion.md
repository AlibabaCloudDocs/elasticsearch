# CancelLogstashDeletion

调用CancelLogstashDeletion，恢复释放后被冻结的Logstash实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=CancelLogstashDeletion&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/actions/cancel-deletion HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-m7r1vsi2\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不值过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|23EBF56B-2DC0-4507-8BE5-B87395DB0FEB|请求ID。 |
|Result|Boolean|true|是否成功恢复实例：

 -   true：是
-   false：否 |

## 示例

请求示例

```
POST /openapi/logstashes/ls-cn-m7r1vsi2****/actions/cancel-deletion HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
  "Result": true,
  "RequestId": "23EBF56B-2DC0-4507-8BE5-B87395DB0FEB"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

