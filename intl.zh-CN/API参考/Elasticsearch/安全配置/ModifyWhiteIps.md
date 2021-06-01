# ModifyWhiteIps

调用ModifyWhiteIps，更新指定实例的访问白名单。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ModifyWhiteIps&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/actions/modify-white-ips HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-0pp1jxvcl000z\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|nodeType|String|Body|否|KIBANA|集群类型。可选值：WORKER（Elasticsearch集群）、KIBANA（Kibana集群）。 |
|networkType|String|Body|否|PRIVATE|网络类型。可选值：PRIVATE（私网）、PUBLIC（公网）。 |
|whiteIpList|List<String\>|Body|否|\["0.0.0.0/0","0.0.0.0/1"\]|白名单列表。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：白名单更新成功
-   false：白名单更新失败 |

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-0pp1jxvcl000z****/actions/modify-white-ips HTTP/1.1
公共请求头
{
   "whiteIpList": [
    "0.0.0.0/0",
    "0.0.0.0/1"
],
"nodeType":"WORKER",
"networkType":"PUBLIC"
}
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "EB03B04E-4700-4EC2-A4D6-9B623F09****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

