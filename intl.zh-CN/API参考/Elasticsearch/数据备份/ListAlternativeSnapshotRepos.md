# ListAlternativeSnapshotRepos

调用ListAlternativeSnapshotRepos，获取当前实例可添加的OSS引用仓库。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListAlternativeSnapshotRepos&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/alternative-snapshot-repos HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-0pp1jxvcl000z\*\*\*\*|实例ID。 |
|alreadySetItems|Boolean|Query|否|true|是否返回已添加的OSS引用仓库。true为默认值，表示返回；false表示不返回。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of repo| |返回结果。 |
|instanceId|String|es-cn-6ja1ro4jt000c\*\*\*\*|实例ID。 |
|repoPath|String|RepoPath|仓库地址。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-0pp1jxvcl000z****/alternative-snapshot-repos HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"instanceId": "es-cn-6ja1ro4jt000c****",
			"repoPath": "es-cn-6ja1ro4jt000c****"
		},
		{
			"instanceId": "es-cn-oew1rgiev0009****",
			"repoPath": "es-cn-oew1rgiev0009****"
		}
	],
	"RequestId": "335D2540-BB16-447F-8AD4-39B7A0AE****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

