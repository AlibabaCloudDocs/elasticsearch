# UninstallPlugin

调用UninstallPlugin，卸载已安装的预置插件。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UninstallPlugin&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/plugins/actions/uninstall HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需要填入待卸载的插件名称，格式为`["pluginname1","pluginname2",…,"plugin_namen"]`，例如`["aliyun-sql","codec-compression"]`。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|List|\["aliyun-mysql"\]|请求卸载的插件列表，失败则异常返回。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/plugins/actions/uninstall HTTP/1.1
公共请求头
["aliyun-sql","codec-compression"]
```

正常返回示例

`XML` 格式

```
<Result>aliyun-mysql</Result>
<RequestId>5A5D8E74-565C-43DC-B031-29289FA9BAD6****</RequestId>
```

`JSON` 格式

```
{
    "Result": ["aliyun-mysql"],
    "RequestId": "5A5D8E74-565C-43DC-B031-29289FA9BAD6****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

