# UpdateXpackMonitorConfig

调用UpdateXpackMonitorConfig，更新Logstash实例的X-Pack监控报警配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateXpackMonitorConfig&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/xpack-monitor-config HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|ClientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定X-Pack监控的配置信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|endpoints

|List<String\\\>

|是

|http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200

|Elasticsearch实例的访问地址。 |
|pipelineIds

|List<String\\\>

|是

|\["name-1","name-2"\]

|待监控的Logstash管道ID列表。 |
|enable

|Boolean

|是

|true

|是否开启X-Pack监控。 |
|userName

|String

|是

|elastic

|Elasticsearch实例的用户名。 |
|password

|String

|是

|\*\*\*\*

|Elasticsearch实例的密码。 |

示例如下。

```

{
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "pipelineIds": ["datahub_test","test"],
    "enable": true,
    "userName": "elastic",
    "password": "xxxx",
    "esInstanceId": "es-cn-n6w1o1x0w001c****"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果。 |

## 示例

请求示例

```
POST /openapi/logstashes/ls-cn-oew1qbgl****/xpack-monitor-config HTTP/1.1
公共请求头
{
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "pipelineIds": ["datahub_test","test"],
    "enable": true,
    "userName": "elastic",
    "password": "xxxx",
    "esInstanceId": "es-cn-n6w1o1x0w001c****"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>30A59FC7-609B-4C12-B6EF-991A5CA7****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "30A59FC7-609B-4C12-B6EF-991A5CA7****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

