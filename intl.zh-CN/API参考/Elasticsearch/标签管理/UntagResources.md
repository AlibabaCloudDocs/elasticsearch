# UntagResources

调用UntagResources，删除用户资源标签关系。

调用该接口时，请注意：

-   只能删除用户标签。

**说明：** 用户标签是指用户手动给实例添加的标签。对应的系统标签是指云产品（即阿里云服务）给用户实例添加的标签。系统标签分为可见标签和不可见标签。

-   对于没有关联任何资源的标签，在删除资源标签关系时，对应标签也要被删除。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UntagResources&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/tags HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ResourceIds|String|是|\["es-cn-09k1rocex0006\*\*\*\*","es-cn-oew1rgiev0009\*\*\*\*"\]|要删除的资源列表。 |
|ResourceType|String|是|INSTANCE|资源类型。固定为**INSTANCE**。 |
|TagKeys|String|否|\["tagKey1","tagKey2"\]|要删除的标签列表，最多包含20个子项。 |
|All|Boolean|否|false|是否全部删除，默认为**false**。仅当**TagKeys**为空时有效。 |

-   当TagKeys为空，且**All = true**时，调用该接口会删除资源下所有的资源标签关系。对于没有标签的资源，不处理接口，并返回成功。
-   当传入**TagKeys**为空，同时**All = false**时，不处理接口，并返回成功。
-   **TagKeys**不为空，**All**无论为true或者false，调用该接口会忽略此字段。
-   指定**TagKeys**后，调用该接口会删除资源上指定的标签。当资源上不存在指定标签时，将不处理该标签。
-   如果资源不存在，调用该接口会返回**InvalidResourceId.NotFound**。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6D\*\*\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：删除成功
-   false：删除失败 |

## 示例

请求示例

```
DELETE /openapi/tags?ResourceType=INSTANCE&ResourceIds=%5B%22es-cn-09k1rocex0006****%22%5D HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>3D8795D9-8FF5-46B2-86E6-E3B40*******</RequestId>
```

`JSON` 格式

```
{
    "Result":true,
    "RequestId": "3D8795D9-8FF5-46B2-86E6-E3B40*******"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

