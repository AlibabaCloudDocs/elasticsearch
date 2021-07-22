# DeleteVpcEndpoint

调用DeleteVpcEndpoint，删除服务vpc下的终端节点。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DeleteVpcEndpoint&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/instances/[InstanceId]/vpc-endpoints/[EndpointId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|EndpointId|String|Path|是|ep-bp18s6wy9420wdi4\*\*\*\*|要删除的终端节点ID。 |
|InstanceId|String|Path|是|es-cn-2r429tctl000d\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不值过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC47D9|请求ID。 |
|Result|Boolean|true|是否删除成功，取值含义如下：

 -   true：成功删除。
-   flase：删除失败。 |

## 示例

请求示例

```
DELETE /openapi/instances/es-cn-2r429tctl000d****/vpc-endpoints/ep-bp18s6wy9420wdi4**** HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC47D9", 
    "Result": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

