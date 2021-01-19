# ListAckNamespaces

调用ListAckNamespaces，查看指定容器服务Kubernetes版ACK（Container Service for Kubernetes）集群的所有命名空间。

**说明：** 创建基于ACK集群的采集器时，需要指定集群的命名空间。您可以调用此接口，查看该集群的所有命名空间，以此为依据来选择合适的命名空间。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListAckNamespaces&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/ack-clusters/[ClusterId]/namespaces HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ClusterId|String|Path|是|c79acd3fbf462423fb6450e513bb6\*\*\*\*|目标集群ID。 |
|page|Integer|Query|否|1|设置返回结果页数。 |
|size|Integer|Query|否|10|每页包含的记录数。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|95789100-A329-473B-9D14-9E0B7DB4BD5A|请求ID。 |
|Result|Array of Result| |返回结果。 |
|namespace|String|logging|集群的命名空间。 |
|status|String|Active|命名空间状态。 |

返回数据中还包含以下参数。

|名称

|类型

|示例值

|描述 |
|----|----|-----|----|
|Headers

|Struct

| |返回头信息。 |
|└X-Total-Count

|Integer

|5

|返回的记录数。 |

**说明：** └表示子参数。

## 示例

请求示例

```
GET /openapi/ack-clusters/c79acd3fbf462423fb6450e513bb6****/namespaces HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <namespace>arms-prom</namespace>
    <status>Active</status>
</Result>
<Result>
    <namespace>default</namespace>
    <status>Active</status>
</Result>
<Result>
    <namespace>kube-node-lease</namespace>
    <status>Active</status>
</Result>
<Result>
    <namespace>kube-public</namespace>
    <status>Active</status>
</Result>
<Result>
    <namespace>kube-system</namespace>
    <status>Active</status>
</Result>
<Result>
    <namespace>logging</namespace>
    <status>Active</status>
</Result>
<RequestId>95789100-A329-473B-9D14-9E0B7DB4BD5A</RequestId>
<Headers>
    <X-Total-Count>6</X-Total-Count>
</Headers>
```

`JSON`格式

```
{
	"Result": [
		{
			"namespace": "arms-prom",
			"status": "Active"
		},
		{
			"namespace": "default",
			"status": "Active"
		},
		{
			"namespace": "kube-node-lease",
			"status": "Active"
		},
		{
			"namespace": "kube-public",
			"status": "Active"
		},
		{
			"namespace": "kube-system",
			"status": "Active"
		},
		{
			"namespace": "logging",
			"status": "Active"
		}
	],
	"RequestId": "95789100-A329-473B-9D14-9E0B7DB4BD5A",
	"Headers": {
		"X-Total-Count": 6
	}
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

