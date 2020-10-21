# GetSuggestShrinkableNodes

调用GetSuggestShrinkableNodes，指定节点类型和数量，获取可缩容的节点。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=GetSuggestShrinkableNodes&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/suggest-shrinkable-nodes HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|count|Integer|是|1|期望获取可缩容节点的数量。 |
|InstanceId|String|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|nodeType|String|是|WORKER|需要缩容的节点类型。**WORKER**表示热节点，**WORKER\_WARM**表示冷节点。 |
|ignoreStatus|Boolean|否|false|是否忽略实例状态，默认为false。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|host|String|192.168.\*\*.\*\*|节点的IP地址。 |
|port|Integer|9200|节点的访问端口号。 |

Result中还包含以下参数。

|名称

|类型

|示例值

|描述 |
|----|----|-----|----|
|nodeType

|String

|WORKER

|节点类型，取值包括：MASTER（专有主节点）、WORKER（热节点）、WORKER\_WARM（冷节点）、COORDINATING（协调节点）、KIBANA（Kibana节点）。 |
|zoneId

|String

|cn-hangzhou-b

|节点所在的可用区ID。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/suggest-shrinkable-nodes?nodeType=WORKER&count=1 HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <nodeType>WORKER</nodeType>
    <host>172.16.**.**</host>
    <port>9200</port>
    <zoneId>cn-hangzhou-i</zoneId>
</Result>
<RequestId>042E33B2-6FB3-474D-BD44-DBE706A4****</RequestId>
```

`JSON` 格式

```
{
	"Result": [
		{
			"nodeType": "WORKER",
			"host": "172.16.**.**",
			"port": 9200,
			"zoneId": "cn-hangzhou-i"
		}
	],
	"RequestId": "042E33B2-6FB3-474D-BD44-DBE706A4****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

