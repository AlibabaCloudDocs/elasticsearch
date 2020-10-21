# ListTagResources

调用ListTagResources，查询可见的资源标签关系。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListTagResources&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/tags HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ResourceType|String|是|INSTANCE|资源类型定义。 |
|Page|Integer|否|1|资源关系列表的分页数。 |
|NextToken|String|否|1d2db86sca4384811e0b5e8707e\*\*\*\*\*\*|下一个查询开始的Token。 |
|Size|Integer|否|10|分页查询时设置的每页条数。 |
|ResourceIds|String|否|\["es-cn-aaa","es-cn-bbb"\]|要查询的实例ID列表。 |
|Tags|String|否|\[\{"key":"env","value","dev"\},\{"key":"dev", "value":"IT"\}\]|要查询的Tags列表，采用JSON字符串的形式，最多包含20个子项。 |

-   您只能在**ResourceIds**和**Tags**参数中，选择一个传入值，否则报错。
-   您只能查询可见标签，不能查询不可见标签。

**说明：** 系统标签是指云产品（即阿里云服务）给用户实例添加的标签。系统标签分为可见标签和不可见标签。


## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。该参数为空，仅供参考，程序中不能强制依赖此参数。

 **说明：** 返回示例中不包含此参数。 |
|X-Total-Count|Integer|10|查询到TagResource的资源数量。 |
|PageSize|Integer|1|分页数。 |
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6D\*\*\*\*\*\*|请求ID。 |
|TagResources|Struct| |标签资源组。 |
|TagResource|Array of TagResource| |标签资源。 |
|ResourceId|String|es-cn-oew1q8bev0002\*\*\*\*|资源ID。 |
|ResourceType|String|ALIYUN::ELASTICSEARCH::INSTANCE|资源类型。固定为`ALIYUN::ELASTICSEARCH::INSTANCE`。 |
|TagKey|String|env|标签键。 |
|TagValue|String|dev|标签值。 |

## 示例

请求示例

```
GET /openapi/tags?ResourceType=INSTANCE HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<RequestId>A5423B74-DBD5-4714-BF28-6725E5BE****</RequestId>
<TagResources>
    <TagResource>
        <ResourceType>ALIYUN::ELASTICSEARCH::INSTANCE</ResourceType>
        <ResourceId>es-cn-oew1q8bev0002****</ResourceId>
        <TagKey>env</TagKey>
        <TagValue>dev</TagValue>
    </TagResource>
</TagResources>
```

`JSON` 格式

```
{
	"RequestId": "A5423B74-DBD5-4714-BF28-6725E5BE****",
	"TagResources": {
		"TagResource": [
			{
				"ResourceType": "ALIYUN::ELASTICSEARCH::INSTANCE",
				"ResourceId": "es-cn-oew1q8bev0002****",
				"TagKey": "env",
				"TagValue": "dev"
			}
		]
	}
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

