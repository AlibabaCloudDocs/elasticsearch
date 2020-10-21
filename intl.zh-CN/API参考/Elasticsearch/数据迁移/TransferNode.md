# TransferNode

调用TransferNode，执行数据迁移任务。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=TransferNode&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/transfer HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|nodeType|String|是|WORKER|节点类型。**WORKER**表示热节点，**WORKER\_WARM**表示冷节点。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需要填入以下参数，用来指定要进行数据迁移任务的节点的IP地址和端口号。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|host

|String

|是

|192.168.xx.xx

|节点的IP地址。 |
|port

|Integer

|是

|9200

|节点的访问端口号。 |

示例如下。

```

[
	{
		"host": "192.168.**.**",
		"port": 9200
	},
	{
		"host": "192.168.**.**",
		"port": 9200
	}
]

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：任务执行成功
-   false：任务执行失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-nif1q9o8r0008****/actions/transfer?nodeType=WORKER HTTP/1.1
公共请求头
[
	{
		"host": "192.168.**.**",
		"port": 9200
	},
	{
		"host": "192.168.**.**",
		"port": 9200
	}
]
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>C82758DD-282F-4D48-934F-92170A33****</RequestId>
```

`JSON` 格式

```
{
    "Result": true,
    "RequestId": "C82758DD-282F-4D48-934F-92170A33****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

