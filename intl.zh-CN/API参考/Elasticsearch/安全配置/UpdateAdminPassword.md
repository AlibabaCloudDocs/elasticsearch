# UpdateAdminPassword

调用UpdateAdminPassword，更新指定Elasticsearch实例的elastic账号的密码。

调用该接口时，请注意：

实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法更新信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateAdminPassword&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/admin-pwd HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入**esAdminPassword**参数，用来指定更新后的密码，示例如下。

```

{
  "esAdminPassword": "es_password*"
}

```

**说明：** 密码长度为8~30个字符，必须同时包含三项：大写字母、小写字母、特殊字符：!@\#$%^&\*\(\)\_+-=。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|0FA05123-745C-42FD-A69B-AFF48EF9\*\*\*\*|请求ID。 |

返回数据中还包含Result参数，即请求的返回结果。为true时，表示elastic账号的密码更新成功；为false时，表示更新失败。

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-nif1q9o8r0008****/admin-pwd HTTP/1.1
公共请求头
{
  "esAdminPassword": "es_password*"
}
```

正常返回示例

`JSON`格式

```
{
    "Result": true,
    "RequestId": "0FA05123-745C-42FD-A69B-AFF48EF9****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

