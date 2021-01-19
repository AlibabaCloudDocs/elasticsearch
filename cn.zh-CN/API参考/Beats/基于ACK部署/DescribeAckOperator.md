# DescribeAckOperator

调用DescribeAckOperator，查看指定容器服务Kubernetes版ACK（Container Service for Kubernetes）集群上安装的Elasticsearch Operator信息。

**说明：** 在ACK集群上安装采集器前，可以先调用该接口，查看目标集群上Elasticsearch Operator的安装状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeAckOperator&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/ack-clusters/[ClusterId]/operator HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ClusterId|String|Path|是|c79acd3fbf462423fb6450e513bb6\*\*\*\*|目标集群ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|6615EE8D-FD9D-4FD3-997E-6FEA5B8D82ED|请求ID。 |
|Result|Struct| |返回结果。 |
|status|String|deployed|Operator安装状态。支持：

 -   deployed：已安装
-   not-deploy：未安装
-   failed：安装失败
-   unknown：未知状态 |
|version|String|1|Operator版本。 |

## 示例

请求示例

```
GET /openapi/ack-clusters/c79acd3fbf462423fb6450e513bb6****/operator HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <version>1</version>
    <status>deployed</status>
</Result>
<RequestId>6615EE8D-FD9D-4FD3-997E-6FEA5B8D82ED</RequestId>
```

`JSON`格式

```
{
	"Result": {
		"version": "1",
		"status": "deployed"
	},
	"RequestId": "6615EE8D-FD9D-4FD3-997E-6FEA5B8D82ED"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

