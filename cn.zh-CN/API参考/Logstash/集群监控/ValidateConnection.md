# ValidateConnection

调用ValidateConnection，在Logstash实例的监控报警配置中，验证提供X-Pack监控的Elasticsearch实例的联通性。

**说明：** 开启Logstash的X-Pack监控需配置Elasticsearch实例。配置后，即可在对应Elasticsearch实例的Kibana中监控Logstash实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ValidateConnection&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/validate-connection HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|ClientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定需要连通的Elasticsearch实例的信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|endpoints

|List<String\\\>

|是

|\["http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]

|提供X-Pack监控的Elasticsearch实例的访问地址。 |
|userName

|String

|是

|elastic

|Elasticsearch实例的用户名。 |
|password

|String

|是

|xxx

|Elasticsearch实例的密码。 |

示例如下。

```

{
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "userName": "elastic",
    "password": "xxxx"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：验证成功
-   false：验证失败 |

## 示例

请求示例

```
POST /openapi/logstashes/ls-cn-oew1qbgl****/validate-connection HTTP/1.1
公共请求头
{
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "userName": "elastic",
    "password": "xxxx"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>D5B41051-FE06-4986-9D87-3779E627****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "D5B41051-FE06-4986-9D87-3779E627****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

