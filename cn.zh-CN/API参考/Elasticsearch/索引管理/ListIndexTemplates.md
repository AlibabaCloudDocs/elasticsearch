# ListIndexTemplates

调用ListIndexTemplates，查询索引模板列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListIndexTemplates&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/index-templates HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif24adwc0082\*\*\*\*|实例ID。 |
|indexTemplate|String|Query|否|my-template|索引模板名称。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Array of Result| |索引模板列表信息。 |
|dataStream|Boolean|true|是否开启数据流，参数取值如下：

 -   true：开启。
-   false（默认值）：不开启。 |
|ilmPolicy|String|my\_ilm\_policy|索引生命周期策略名称。 |
|indexPatterns|List|console-\*|索引模式。 |
|indexTemplate|String|my-template|索引模版名称。 |
|priority|Integer|100|索引模板优先级。 |
|template|Struct| |组件模版。 |
|aliases|String|\{\\"index.number\_of\_shards\\":\\"1\\"\}|aliases设置。 |
|mappings|String|\{\\"properties\\":\{\\"created\_at\\":\{\\"format\\":\\"EEE MMM dd HH:mm:ss Z yyyy\\",\\"type\\":\\"date\\"\},\\"host\_name\\":\{\\"type\\":\\"keyword\\"\}\}\}|mappings设置。 |
|settings|String|\{\\"mydata\\":\{\}\}|settings设置。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-nif24adwc0082****/index-templates?indexTemplate=my-template HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC****",
    "Result": {
        "indexTemplate": "my-template",
        "indexPatterns": "console-*",
        "dataStream": true,
        "priority": 100,
        "ilmPolicy": "my_ilm_policy",
        "template": {
            "settings": "{\"index.number_of_shards\":\"1\"}",
            "mappings": "{\"properties\":{\"created_at\":{\"format\":\"EEE MMM dd HH:mm:ss Z yyyy\",\"type\":\"date\"},\"host_name\":{\"type\":\"keyword\"}}}",
            "aliases": "{\"mydata\":{}}"
        }
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

