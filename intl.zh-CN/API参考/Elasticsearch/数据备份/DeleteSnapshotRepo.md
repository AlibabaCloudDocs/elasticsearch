# DeleteSnapshotRepo

调用DeleteSnapRepo，删除一个跨集群OSS引用仓库。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DeleteSnapshotRepo&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/instances/[InstanceId]/snapshot-repos HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|repoPath|String|是|es-cn-n6w1rux8i000w\*\*\*\*|引用实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果。 |

## 示例

请求示例

```
DELETE /openapi/instances/es-cn-n6w1o1x0w001c****/snapshot-repos?repoPath=es-cn-n6w1rux8i000w**** HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>93005691-1899-4515-A5CE-FD28D347****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "93005691-1899-4515-A5CE-FD28D347****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

