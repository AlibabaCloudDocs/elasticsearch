# ListConnectedClusters

调用ListConnectedClusters，获取已经与当前实例进行了网络互通的实例列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListConnectedClusters&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/connected-clusters HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-0pp1jxvcl000z\*\*\*\*|当前实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|Result| | | |
|instances|String|es-cn-09k1rocex0006\*\*\*\*|已经与当前实例进行网络互通的远程实例ID。 |
|networkType|String|vpc|实例的网络类型。 |

## 示例

请求示例

```
GET /openapi/instances/[InstanceId]/connected-clusters HTTP/1.1
公共请求头
{
"InstanceId": "es-cn-0pp1jxvcl000z****"
}
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>es-cn-09k1rocex0006****</instanceId>
    <networkType>vpc</networkType>
</Result>
<RequestId>8D6EE77E-D56E-4E88-A8EE-407B77FB****</RequestId>
```

`JSON` 格式

```
{
	"Result": [
		{
			"instanceId": "es-cn-09k1rocex0006****",
			"networkType": "vpc"
		}
	],
	"RequestId": "8D6EE77E-D56E-4E88-A8EE-407B77FB****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

