# DescribeKibanaSettings

调用DescribeKibanaSettings，获取Kibana配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeKibanaSettings&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/kibana-settings HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6D\*\*\*\*\*|请求ID。 |
|Result|Map|\{"map.includeElasticMapsService": "false", "server.ssl.cert": "/home/admin/packages/kibana/config/cert/client.crt", "server.ssl.enabled": "true", "server.ssl.key": "/home/admin/packages/kibana/config/cert/client.key", "xpack.reporting.capture.browser.chromium.disableSandbox": "true"\}|返回结果：

 -   true：获取Kibana配置成功
-   false：获取Kibana配置失败 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/kibana-settings HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <map.includeElasticMapsService>false</map.includeElasticMapsService>
    <server.ssl.cert>/home/admin/packages/kibana/config/cert/client.crt</server.ssl.cert>
    <server.ssl.enabled>true</server.ssl.enabled>
    <server.ssl.key>/home/admin/packages/kibana/config/cert/client.key</server.ssl.key>
    <xpack.reporting.capture.browser.chromium.disableSandbox>true</xpack.reporting.capture.browser.chromium.disableSandbox>
</Result>
<RequestId>131834B6-AE89-45D4-878B-D2F46A8C****</RequestId>
```

`JSON` 格式

```
{
	"Result": {
		"map.includeElasticMapsService": "false",
		"server.ssl.cert": "/home/admin/packages/kibana/config/cert/client.crt",
		"server.ssl.enabled": "true",
		"server.ssl.key": "/home/admin/packages/kibana/config/cert/client.key",
		"xpack.reporting.capture.browser.chromium.disableSandbox": "true"
	},
	"RequestId": "131834B6-AE89-45D4-878B-D2F46A8C****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

