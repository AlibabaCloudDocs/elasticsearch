# CreateDataStream

调用CreateDataStream，创建数据流。

**说明：** 您创建的数据流名称需要与索引模板中的索引模式一一对应，且该索引模板已开启数据流。例如：索引模板中的索引模式为ds-\*，则对应的数据流名称应该为ds-。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=CreateDataStream&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/data-streams HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif24adwc0082\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

RequestBody中需要填入以下参数，用来指定待创建数据流。

|参数

|类型

|是否必填

|示例值

|描述 |
|----|----|------|-----|----|
|name

|String

|是

|ds-

|数据流名称。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Struct| |返回创建的数据流名称。 |
|name|String|ds-|数据流名称。 |

**说明：**

-   当系统返回【DatastreamUnmatchedIndexTemplateError】消息时，表示数据流没有匹配的索引模板，请修改数据流名称，或前往索引模板管理进行创建或修改。
-   当系统返回【DatastreamDuplicatedError】消息时，表示数据流已经存在时，请重新命名，或前往索引模板管理进行创建或修改。

## 示例

请求示例

```
POST /openapi/instances/es-cn-nif24adwc0082****/data-streams HTTP/1.1
公共请求头
{
  "name": "ds-"
}
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC****",
    "Result": {
        "name": "ds-"
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

