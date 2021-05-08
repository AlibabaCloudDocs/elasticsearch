# DeleteConnectedCluster

调用DeleteConnectedCluster，移除互通实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DeleteConnectedCluster&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/instances/[InstanceId]/connected-clusters HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|connectedInstanceId|String|Query|是|es-cn-09k1rgid9000g\*\*\*\*|已进行网络互通的远程实例ID。 |
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|当前实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：移除互通实例成功
-   false：移除互通实例失败 |

## 示例

请求示例

```
DELETE /openapi/instances/es-cn-n6w1o1x0w001c****/connected-clusters?connectedInstanceId=es-cn-09k1rgid9000g**** HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "4EA9579E-12E6-420B-9518-575FFC76****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

