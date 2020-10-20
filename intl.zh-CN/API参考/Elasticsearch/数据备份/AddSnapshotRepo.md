# AddSnapshotRepo

调用AddSnapshotRepo，在设置跨集群OSS仓库时，创建引用仓库。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=AddSnapshotRepo&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。

## 请求语法

```
POST /openapi/instances/[InstanceId]/snapshot-repos HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|需要设置跨集群OSS仓库的实例ID。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定跨集群备份信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|repoPath

|String

|是

|es-cn-4591jumei000u\*\*\*\*

|需要恢复数据的实例。指定后，Elasticsearch会为您创建该实例的快照引用仓库，您可以从该快照仓库进行数据恢复。

 该实例需要与源实例满足以下条件：

 相同区域；归属于相同账号；源端实例的版本低于或等于目标端实例的版本，更多限制详情请参见[设置跨集群OSS仓库](~~131441~~)。 |

示例如下。

```

{
    "repoPath" :"es-cn-4591jumei000u****"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：引用仓库创建成功
-   false：引用仓库创建失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/snapshot-repos HTTP/1.1
{
    "repoPath" :"es-cn-4591jumei000u****"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>D21379E3-A54E-4C86-A64C-3717365F****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "D21379E3-A54E-4C86-A64C-3717365F****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

