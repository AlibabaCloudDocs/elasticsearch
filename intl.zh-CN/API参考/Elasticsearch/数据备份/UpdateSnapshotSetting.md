# UpdateSnapshotSetting

调用UpdateSnapshotSetting，更新指定实例的数据备份配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateSnapshotSetting&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST|PUT /openapi/instances/[InstanceId]/snapshot-setting HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-oew1rgiev0009\*\*\*\*|实例ID。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定修改后的数据备份信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|quartzRegex

|String

|否

|0 0 01 ? \* \* \*

|自动备份开始时间。当enable为true时，必填。 |
|enable

|Boolean

|是

|true

|是否开启定时备份。 |

示例如下。

```

{
    "quartzRegex":"0 0 01 ? * * *",
    "enable":true
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|enable|Boolean|true|是否开启自动备份。 |
|quartzRegex|String|0 0 01 ? \* \* \*|自动备份开始时间。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-oew1rgiev0009****/snapshot-setting HTTP/1.1
公共请求头
{
    "quartzRegex":"0 0 01 ? * * *",
    "enable":true
}
```

正常返回示例

`XML` 格式

```
<Result>
    <quartzRegex>0 0 01 ? * * *</quartzRegex>
    <enable>true</enable>
</Result>
<RequestId>77C0C894-77E3-4711-88E1-495216FA****</RequestId>
```

`JSON` 格式

```
{
	"Result": {
		"quartzRegex": "0 0 01 ? * * *",
		"enable": true
	},
	"RequestId": "77C0C894-77E3-4711-88E1-495216FA****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

