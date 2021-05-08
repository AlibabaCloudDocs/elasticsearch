# DescribeIndexTemplate

调用DescribeIndexTemplate，查看组件索引模版详情，包括索引生命周期。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeIndexTemplate&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/index-templates/[IndexTemplate] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|IndexTemplate|String|Path|是|data-stream-default|索引模版名称。 |
|InstanceId|String|Path|是|es-cn-n6w24n9u900am\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|25DB38F8-82E4-4D16-82BB-FF077C7F\*\*\*\*|请求ID。 |
|Result|Struct| |返回索引模板详情。 |
|dataStream|Boolean|true|是否开启数据流：

 -   true：开启
-   false：不开启

 默认值：false（不开启）。 |
|ilmPolicy|String|cube\_default\_ilm\_policy|生命周期策略名称。 |
|indexPatterns|List|ds-\*|索引匹配模式正则。 |
|indexTemplate|String|data-stream-default|索引模版名称。 |
|priority|Integer|0|优先级。 |
|template|Struct| |组件模版。 |
|aliases|String|\{\\"mydata\\":\{\}\}|aliases设置。 |
|mappings|String|\{\\"properties\\":\{\\"created\_at\\":\{\\"format\\":\\"EEE MMM dd HH:mm:ss Z yyyy\\",\\"type\\":\\"date\\"\},\\"host\_name\\":\{\\"type\\":\\"keyword\\"\}\}\}|mappings设置。 |
|settings|String|\{\\"index.refresh\_interval\\":\\"1s\\"\}|settings设置。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w24n9u900am****/index-templates/data-stream-default HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "25DB38F8-82E4-4D16-82BB-FF077C7F****",
    "Result": {
        "ilmPolicy": "cube_default_ilm_policy",
        "dataStream": true,
        "indexTemplate": "data-stream-default",
        "priority": 0,
        "indexPatterns": "ds-*",
        "template": {
            "settings": "{\"index.refresh_interval\":\"1s\"}",
            "mappings": "{\"properties\":{\"created_at\":{\"format\":\"EEE MMM dd HH:mm:ss Z yyyy\",\"type\":\"date\"},\"host_name\":{\"type\":\"keyword\"}}}",
            "aliases": "{\"mydata\":{}}"
        }
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

