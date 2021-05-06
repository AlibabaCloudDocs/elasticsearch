# UninstallLogstashPlugin

调用UninstallLogstashPlugin，卸载已安装的插件。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UninstallLogstashPlugin&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/plugins/actions/uninstall HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入待卸载的插件的名称，格式为`["pluginName1","pluginName2",...,"pluginNamen"]`，示例如下。

```

[
    "logstash-input-datahub",
    "logstash-input-maxcompute"
]

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|List|\["logstash-input-datahub", "logstash-input-maxcompute" \]|返回结果：

 -   true：卸载成功
-   false：卸载失败 |

返回数据中还包含Headers参数，表示返回头信息。

## 示例

请求示例

```
POST /openapi/logstashes/ls-cn-oew1qbgl****/plugins/actions/uninstall HTTP/1.1
公共请求头
[
    "logstash-input-datahub", "logstash-input-maxcompute"
]
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		"logstash-input-datahub",
		"logstash-input-maxcompute"
	],
	"RequestId": "63EB9CD3-7652-4CFD-B38C-6413BCEC****",
	"Headers": {}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

