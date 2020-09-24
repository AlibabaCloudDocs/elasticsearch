# DescribeConnectableClusters

调用DescribeConnectableClusters，获取能够与当前实例进行网络互通的实例列表。不包括已经打通的实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeConnectableClusters&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。

## 请求语法

```
GET /openapi/instances/[InstanceId]/connectable-clusters HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|当前实例ID。 |
|alreadySetItems|Boolean|否|true|是否返回已经互通的实例。ture为默认值，表示返回的实例列表中包括已经互通的实例；false表示不包括。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of ConnectableClustersInfo| |返回结果。 |
|instances|String|es-cn-xxx|可以进行网络互通的实例ID。 |
|networkType|String|vpc|实例的网络类型。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/connectable-clusters HTTP/1.1
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>es-cn-09k1rgid9000g****</instanceId>
    <networkType>vpc</networkType>
</Result>
<RequestId>506B7495-0887-4F05-BC76-5AE7AC1D****</RequestId>
```

`JSON` 格式

```
{
	"Result": [
		{
			"instanceId": "es-cn-09k1rgid9000g****",
			"networkType": "vpc"
		}
	],
	"RequestId": "506B7495-0887-4F05-BC76-5AE7AC1D****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

