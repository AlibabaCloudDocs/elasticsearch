# UninstallKibanaPlugin

调用UninstallKibanaPlugin，卸载Kibana插件。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UninstallKibanaPlugin&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/kibana-plugins/actions/uninstall HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-6ja1ro4jt000c\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需要填入待卸载的Kibana插件名称，格式为`["pluginname1","pluginname2",…,"plugin_namen"]`，例如`["bsearch_label","bsearch_querybuilder"]`。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|List|\["bsearch\_querybuilder"\]|返回结果，卸载的插件列表。 |

**说明：** 返回数据中还包含Headers参数，表示请求头信息。

## 示例

请求示例

```
POST /openapi/instances/es-cn-6ja1ro4jt000c****/kibana-plugins/actions/uninstall HTTP/1.1
公共请求头
["bsearch_label","bsearch_querybuilder"]
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		"bsearch_querybuilder"
	],
	"RequestId": "D528727E-F512-4EE6-B46F-B9270D4E****",
	"Headers": {}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

