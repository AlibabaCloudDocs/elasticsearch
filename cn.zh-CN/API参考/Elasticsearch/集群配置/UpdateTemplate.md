# UpdateTemplate

调用UpdateTemplate，修改集群的场景化模板配置内容。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateTemplate&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/templates/[TemplateName] HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClientToken|String|是|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|TemplateName|String|是|dynamicSettings|模板名称。支持：

 -   dynamicSettings：集群动态配置
-   indexTemplate：索引模板配置
-   ilmPolicy：索引生命周期配置 |

## RequestBody

RequestBody中还需填入以下参数，用来指定模板配置内容。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|content

|String

|是

|\{\\n\\t\\"persistent\\":\{\\n\\t\\t\\"search\\":\{\\n\\t\\t\\t\\"max\_buckets\\":\\"10000\\"\\n\\t\\t\}\\n\\t\}\\n\}

|模板配置内容。 |

示例如下。

```

{
    "content": "{\n\t\"persistent\":{\n\t\t\"search\":{\n\t\t\t\"max_buckets\":\"10000\"\n\t\t}\n\t}\n}"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/templates/dynamicSettings HTTP/1.1
公共请求头
{
    "content": "{\n\t\"persistent\":{\n\t\t\"search\":{\n\t\t\t\"max_buckets\":\"10000\"\n\t\t}\n\t}\n}"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>7716D5C1-1750-4096-98B6-1CE533BA****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "7716D5C1-1750-4096-98B6-1CE533BA****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

