# ListAckClusters

调用ListAckClusters，获取容器服务Kubernetes版ACK（Container Service for Kubernetes）集群列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListAckClusters&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/ack-clusters HTTPS|HTTP
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|page|Integer|Query|否|3|设置返回结果的页数。 |
|size|Integer|Query|否|20|每页包含的结果数。 |
|vpcId|String|Query|否|vpc-bp12nu14urf0upaf4\*\*\*\*|ACK集群所在的专有网络ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F93EAA49-284F-4FCE-9E67-FA23FB4BB512|请求ID。 |
|Result|Array of result| |返回结果。 |
|clusterId|String|c5ea2c2d9a3cf499481292f60425d\*\*\*\*|集群ID。 |
|clusterType|String|ManagedKubernetes|集群类型，仅支持ManagedKubernetes，即Kubernetes集群。 |
|name|String|test|集群名称。 |
|vpcId|String|vpc-bp12nu14urf0upaf4\*\*\*\*|集群所在的专有网络ID。 |

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

|2

|返回的记录数。 |

**说明：** └表示子参数。

## 示例

请求示例

```
GET /openapi/ack-clusters?page=1&size=20 HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <clusterId>c5ea2c2d9a3cf499481292f60425d****</clusterId>
    <name>zl-keepit</name>
    <clusterType>ManagedKubernetes</clusterType>
    <vpcId>vpc-bp12nu14urf0upaf4****</vpcId>
</Result>
<Result>
    <clusterId>cdcee8be4e87a40e2a23fdbc1c24d****</clusterId>
    <name>cs</name>
    <clusterType>ManagedKubernetes</clusterType>
    <vpcId>vpc-bp16k1dvzxtmagcva****</vpcId>
</Result>
<RequestId>F93EAA49-284F-4FCE-9E67-FA23FB4BB512</RequestId>
<Headers>
    <X-Total-Count>2</X-Total-Count>
</Headers>
```

`JSON`格式

```
{
	"Result": [
		{
			"clusterId": "c5ea2c2d9a3cf499481292f60425d****",
			"name": "test",
			"clusterType": "ManagedKubernetes",
			"vpcId": "vpc-bp12nu14urf0upaf4****"
		},
		{
			"clusterId": "cdcee8be4e87a40e2a23fdbc1c24d****",
			"name": "cs",
			"clusterType": "ManagedKubernetes",
			"vpcId": "vpc-bp16k1dvzxtmagcva****"
		}
	],
	"RequestId": "F93EAA49-284F-4FCE-9E67-FA23FB4BB512",
	"Headers": {
		"X-Total-Count": 2
	}
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

