# UpdatePipelineManagementConfig

调用UpdatePipelineManagementConfig，更新Logstash管道管理方式。

**说明：** 管道管理方式分为配置文件管理和Kibana管道管理，目前控制台已不支持Kibana管道管理，仅可通过API使用此功能。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdatePipelineManagementConfig&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/pipeline-management-config HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定关联的Elasticsearch实例信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|esInstanceId

|String

|是

|es-cn-n6w1o1x0w001c\*\*\*\*

|开启Kibana管理管道后，Kibana所在的Elasticsearch实例的ID。 |
|userName

|String

|是

|elastic

|Kibana的用户名。 |
|password

|String

|是

|xxxxxx

|Kibana的密码。 |
|pipelineIds

|List<String\\\>

|是

|\["testKibanaManagement"\]

|Kibana管理的管道列表。 |
|endpoints

|List<String\\\>

|是

|\["http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]

|开启Kibana管理管道后，Kibana所在Elasticsearch实例的访问地址列表。 |
|pipelineManagementType

|String

|是

|ES

|管道管理方式，支持：ES（Kibana管道管理）、MULTIPLE\_PIPELINE（配置文件管理）。 |

示例如下。

```

{
    "pipelineManagementType": "ES",
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "pipelineIds": [
        "testKibanaManagement"
    ],
    "userName": "elastic",
    "password": "xxxx",
    "esInstanceId": "es-cn-n6w1o1x0w001c****"
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
POST /openapi/logstashes/ls-cn-oew1qbgl****/pipeline-management-config HTTP/1.1
公共请求头
{
    "pipelineManagementType": "ES",
    "endpoints": [
        "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
    ],
    "pipelineIds": [
        "testKibanaManagement"
    ],
    "userName": "elastic",
    "password": "xxxx",
    "esInstanceId": "es-cn-n6w1o1x0w001c****"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>135E9F19-277D-4E34-85AC-EB394AA2****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "135E9F19-277D-4E34-85AC-EB394AA2****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

