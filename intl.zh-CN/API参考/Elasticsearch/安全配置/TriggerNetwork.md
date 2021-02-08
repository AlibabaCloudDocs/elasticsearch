# TriggerNetwork

调用TriggerNetwork，开启或关闭Elasticsearch或Kibana集群的公网或私网访问。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=TriggerNetwork&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/network-trigger HTTPS|HTTP
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|nodeType|String|Body|否|KIBANA|实例类型。可选值：

 -   KIBANA：Kibana集群
-   WORKER：Elasticsearch集群 |
|networkType|String|Body|否|PUBLIC|网络类型。可选值：

 -   PUBLIC：公网
-   PRIVATE：私网 |
|actionType|String|Body|否|OPEN|动作类型。可选值：

 -   CLOSE：关闭
-   OPEN：开启 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/network-trigger HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>5A5D8E74-565C-43DC-B031-29289F****</RequestId>
```

`JSON` 格式

```
{
    "Result": true,
    "RequestId": "5A5D8E74-565C-43DC-B031-29289F****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

