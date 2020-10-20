# DescribeSnapshotSetting

调用DescribeSnapshotSetting，获取集群的数据备份配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeSnapshotSetting&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/snapshot-setting HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-0pp1jxvcl000z\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|Enable|Boolean|true|是否开启自动备份。 |
|QuartzRegex|String|0 0 01 ? \* \* \*|自动备份时间配置，采用Quartz Cron表达式。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-0pp1jxvcl000z****/snapshot-setting HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<RequestId>B2A05BB2-7B66-4D0F-BEDE-5033AFFE****</RequestId>
<Result>
    <Enable>true</Enable>
    <QuartzRegex>0 0 01 ? * * *</QuartzRegex>
</Result>
```

`JSON` 格式

```
{
	"RequestId": "B2A05BB2-7B66-4D0F-BEDE-5033AFFE****",
	"Result": {
		"Enable": true,
		"QuartzRegex": "0 0 01 ? * * *"
	}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|404|InstanceNotFound|The specified cluster does not exist. Check the cluster status and try again.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

