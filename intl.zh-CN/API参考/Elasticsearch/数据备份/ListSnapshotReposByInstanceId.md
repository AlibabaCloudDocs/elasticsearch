# ListSnapshotReposByInstanceId

调用ListSnapshotReposByInstanceId，获取当前实例的跨集群OSS仓库设置列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListSnapshotReposByInstanceId&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/snapshot-repos HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-0pp1jxvcl000z\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Array of repo| |返回结果。 |
|instanceId|String|es-cn-6ja1ro4jt000c\*\*\*\*|引用实例ID。 |
|repoPath|String|es-cn-6ja1ro4jt000c\*\*\*\*|仓库地址。 |
|snapWarehouse|String|aliyun\_snapshot\_from\_es-cn-6ja1ro4jt000c\*\*\*\*|引用仓库名称。 |
|status|String|available|引用仓库状态。available表示生效；unavailable表示失效。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-0pp1jxvcl000z****/snapshot-repos HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>es-cn-6ja1ro4jt000c****</instanceId>
    <snapWarehouse>aliyun_snapshot_from_es-cn-6ja1ro4jt000c****</snapWarehouse>
    <repoPath>es-cn-6ja1ro4jt000c****</repoPath>
    <status>available</status>
</Result>
<RequestId>123BB496-6EEF-41E6-92BB-3F782664****</RequestId>
```

`JSON` 格式

```
{
	"Result": [
		{
			"instanceId": "es-cn-6ja1ro4jt000c****",
			"snapWarehouse": "aliyun_snapshot_from_es-cn-6ja1ro4jt000c****",
			"repoPath": "es-cn-6ja1ro4jt000c****",
			"status": "available"
		}
	],
	"RequestId": "123BB496-6EEF-41E6-92BB-3F782664****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

