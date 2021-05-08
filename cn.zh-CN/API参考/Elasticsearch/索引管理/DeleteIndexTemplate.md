# DeleteIndexTemplate

调用DeleteIndexTemplate，删除索引模板。

**说明：** 删除索引模板前，请先删除关联该索引模板的数据流。否则，将无法删除对应的索引模板。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DeleteIndexTemplate&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/instances/[InstanceId]/index-templates/[IndexTemplate] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|IndexTemplate|String|Path|是|index-name|索引模板名称。 |
|InstanceId|String|Path|是|es-cn-nif24adwc0082\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A0761F7E-0B50-46B9-8CAA-EBB3A420\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：删除索引模板成功。
-   false：删除索引模板失败。 |

## 示例

请求示例

```
DELETE /openapi/instances/es-cn-nif24adwc0082****/index-templates/index-name HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "A0761F7E-0B50-46B9-8CAA-EBB3A420****",
    "Result": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

