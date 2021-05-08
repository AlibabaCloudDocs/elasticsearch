# UpdateIndexTemplate

调用UpdateIndexTemplate，更新索引模版的组件化设置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateIndexTemplate&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/instances/[InstanceId]/index-templates/[IndexTemplate] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|IndexTemplate|String|Path|是|my-template|索引模版名称。 |
|InstanceId|String|Path|是|es-cn-n6w24n9u900am\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需要填入以下参数，用来指定待更新的索引模版信息。

|参数

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|indexTemplate

|String

|是

|my-template

|索引模版名称。必须和path值相同。 |
|indexPatterns

|String

|是

|\["schema1\*","schema2\*"\]

|索引匹配模式正则。 |
|dataStream

|Boolean

|是

|true

|是否开启数据流：true（开启）、false（不开启）。默认值：false（不开启）。 |
|priority

|Integre

|否

|30

|集群索引模板的优先级。 |
|ilmPolicy

|String

|否

|policy-1

|生命周期策略名称。 |
|template

|Object

|否

| |组件模版。 |

template字段数据结构说明

|参数

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|settings

|String

|否

|\{\\"index.refresh\_interval\\":\\"1s\\"\}

|settings设置。 |
|mappings

|String

|否

|\{"properties": \{"created\_at": \{"type": "date","format": "EEE MMM dd HH:mm:ss Z yyyy"\},"host\_name": \{"type": "keyword"\}\}\}

|mappings设置。 |
|aliases

|String

|否

|\{"mydata": \{\}\}

|aliases设置。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|String|my-template|更新的索引模板名称。 |

## 示例

请求示例

```
PUT /openapi/instances/es-cn-n6w24n9u900am****/index-templates/my-template HTTP/1.1
公共请求头
{
  "indexTemplate":"my-template",
  "indexPatterns": ["schema1*","schema2*"],
  "dataStream": true,
  "priority": 30,
  "ilmPolicy":"policy-1",
  "template": {
    "settings": "{\"index.refresh_interval\":\"1s\"}",
    "aliases": "{\"mydata\": {}}",
    "mappings": "{\"properties\":{\"created_at\":{\"format\":\"EEE MMM dd HH:mm:ss Z yyyy\",\"type\":\"date\"},\"host_name\":{\"type\":\"keyword\"}}}"
  }
}
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC****",
    "Result": "my-template"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

