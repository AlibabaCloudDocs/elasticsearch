# UpdateKibanaSettings

调用UpdateKibanaSettings，修改Kibana配置。目前仅支持修改Kibana语言配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateKibanaSettings&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/actions/update-kibana-settings HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B350\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

您还可以在RequestBody中填入**i18n.locale**参数（可选，默认为en），用来设置Kibana语言，示例如下。

```

{
  "i18n.locale":"en"
}

```

**说明：** `i18n.locale`参数只能取en（英文）或zh-CN（中文）。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DC\*\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：Kibana语言修改成功
-   false：Kibana语言修改失败 |

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-n6w1o1x0w001c****/actions/update-kibana-settings HTTP/1.1
公共请求头
{
  "i18n.locale":"en"
}
```

正常返回示例

`JSON`格式

```
{
    "Result": true,
    "RequestId": "5A5D8E74-565C-43DC-B031-29289FA9****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

