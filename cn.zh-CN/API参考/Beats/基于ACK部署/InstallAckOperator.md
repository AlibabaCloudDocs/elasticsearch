# InstallAckOperator

调用InstallAckOperator，在指定容器服务Kubernetes版ACK（Container Service for Kubernetes）集群上安装Elasticsearch Operator。

**说明：** 在ACK集群上安装采集器前，需要先调用该接口，在目标集群上安装Elasticsearch Operator。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=InstallAckOperator&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/ack-clusters/[ClusterId]/operator HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ClusterId|String|Path|是|c79acd3fbf462423fb6450e513bb6\*\*\*\*|目标集群ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|EFA88951-7A6F-4A8E-AB8F-2BB7132BA751|请求ID。 |
|Result|Boolean|true|返回结果。支持：

-   true：安装成功
-   false：安装失败 |

## 示例

请求示例

```
POST /openapi/ack-clusters/c79acd3fbf462423fb6450e513bb6****/operator HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>true</Result>
<RequestId>EFA88951-7A6F-4A8E-AB8F-2BB7132BA751</RequestId>
```

`JSON`格式

```
{
    "Result": true,
    "RequestId": "EFA88951-7A6F-4A8E-AB8F-2BB7132BA751"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

