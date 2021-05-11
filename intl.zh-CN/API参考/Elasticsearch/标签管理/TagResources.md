# TagResources

调用TagResources，创建标签资源关系。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=TagResources&type=ROA&version=2017-06-13)

## 请求头

https://elasticsearch.cn-hangzhou.aliyuncs.com/openapi/tags

## 请求语法

```
POST /openapi/tags HTTP/1.1
```

## 请求参数

请求参数为空，但需填写RequestBody。

RequestBody中需要填入以下参数，用来定义要创建的资源及标签。

|参数

|类型

|是否必须

|示例值

|描述 |
|----|----|------|-----|----|
|ResourceIds

|Array

|是

|\["es-cn-aaa","es-cn-bbb"\]

|资源ID。 |
|Tags

|Array

|是

|\[\{"key":"env","value":"IT"\}\]

|新增的标签列表，最多包含20个子项。 |
|ResourceType

|String

|是

|INSTANCE

|资源类型，固定为INSTANCE。 |

**说明：**

-   除了必传参数，Tags中如果存在key，则必须存在value（可以为空字符串），否则报错InvalidParameter.TagKey。
-   当传入的键值对重复时，会报错Duplicate.TagKey。
-   如果在自定义Tags之前已经存在传入的Tag key，则之前的Tag key的value值会被覆盖。

示例如下。

```

{
    "ResourceIds":["es-cn-oew1q8bev0002****","es-cn-09k1ptccp0009****"],
    "Tags": [
        {
            "key": "env",
            "value": "IT"
        }
    ],
    "ResourceType": "INSTANCE"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|3D8795D9-8FF5-46B2-86E6-E3B407\*\*\*\*\*\*\*|请求ID。 |

返回参数中还包括**Result**参数，为Boolean类型。返回true表示标签资源关系创建成功，返回false表示创建失败。

## 示例

请求示例

```
POST /openapi/tags HTTP/1.1
公共请求头
{
    "ResourceIds":["es-cn-oew1q8bev0002****","es-cn-09k1ptccp0009****"],
    "Tags": [
        {
            "key": "env",
            "value": "IT"
        }
    ],
    "ResourceType": "INSTANCE"
}
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "27627E6B-E26A-406F-B6E1-0247882C****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

