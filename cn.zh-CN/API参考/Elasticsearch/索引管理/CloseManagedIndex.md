# CloseManagedIndex

调用CloseManagedIndex，关闭Indexing Service集群中某个索引的云端托管功能。此功能不可逆，关闭后无法再开启。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=CloseManagedIndex&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/indices/[Index]/close-managed HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|Index|String|Path|是|test-index1|索引名称。 |
|InstanceId|String|Path|是|es-cn-m7r24d27d007q\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：关闭索引的云端托管成功。
-   flase：关闭索引的云端托管失败。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-m7r24d27d007q****/indices/test-index1/close-managed HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<RequestId>F99407AB-2FA9-489E-A259-40CF6DCC****</RequestId>
<Result>true</Result>
```

`JSON`格式

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC****",
    "Result": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

