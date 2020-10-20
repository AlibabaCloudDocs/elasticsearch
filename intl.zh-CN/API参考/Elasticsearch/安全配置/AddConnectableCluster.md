# AddConnectableCluster

调用AddConnectableCluster，配置实例网络互通。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=AddConnectableCluster&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/connected-clusters HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|当前实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定需要进行网络互通的远程实例ID，要求与当前实例在同一专有网络下。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|instanceId

|String

|是

|es-cn-09k1rgid9000g\*\*\*\*

|远程实例ID。 |

示例如下。

```

{
    "instanceId":"es-cn-09k1rgid9000g****"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5A5D8E74-565C-43DC-B031-29289FA\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：配置成功
-   false：配置失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/connected-clusters HTTP/1.1
公共请求头
{
    "instanceId":"es-cn-09k1rgid9000g****"
}
```

正常返回示例

`XML` 格式

```
<RequestId>5A5D8E74-565C-43DC-B031-29289FA****</RequestId>
<Result>true</Result>
```

`JSON` 格式

```
{
    "RequestId": "5A5D8E74-565C-43DC-B031-29289FA****",
    "Result": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

