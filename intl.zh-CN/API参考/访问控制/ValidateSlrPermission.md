# ValidateSlrPermission

调用ValidateSlrPermission，验证是否已经创建服务关联角色。

**说明：** 在通过采集器采集来自不同数据源的日志时，需要先授权创建服务关联角色。您可以调用此接口，验证是否已经创建。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ValidateSlrPermission&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/user/servicerolepermission HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|rolename|String|Query|是|AliyunServiceRoleForElasticsearchCollector|服务关联角色名称。可选值：

 -   AliyunServiceRoleForElasticsearchCollector：创建和管理Beats采集器 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BC4ED7DD-8C84-49B5-8A95-456F82E44D13|请求ID。 |
|Result|Boolean|true|是否已创建服务关联角色。支持：

 -   true：已创建
-   false：未创建 |

## 示例

请求示例

```
GET /openapi/user/servicerolepermission?rolename=AliyunServiceRoleForElasticsearchCollector HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>true</Result>
<RequestId>2C77A9B5-6B2A-42D7-9DBB-0166A0D40483</RequestId>
```

`JSON`格式

```
{
  "Result": true,
  "RequestId": "2C77A9B5-6B2A-42D7-9DBB-0166A0D40483"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

