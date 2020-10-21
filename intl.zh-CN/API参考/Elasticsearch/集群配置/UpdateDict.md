# UpdateDict

调用UpdateDict，更新Elasticsearch实例的用户词典。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateDict&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/instances/[InstanceId]/dict HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数。

|参数

|类型

|是否必须

|示例值

|描述 |
|----|----|------|-----|----|
|name

|String

|是

|dic\_0.dic

|您上传的词典文件名称。 |
|ossObject

| |否

| |OSS的开放存储文件描述。当sourceType为OSS时，必填。 |
|└bucketName

|String

|否

|search-cloud-test-cn-\*\*\*\*

|OSS存储空间名称。 |
|└key

|String

|否

|oss/dic\_0.dic

|词典文件在OSS中的存储路径。 |
|sourceType

|String

|是

|OSS

|同义词来源类型，支持：OSS（OSS开放存储）、ORIGIN（开源Elasticsearch）、UPLOAD（上传的文件）。如果为OSS，需要确保OSS存储空间为公共可读。 |
|type

|String

|是

|MAIN

|词典类型，支持：STOP（停用词）、MAIN（主词典）、SYNONYMS（同义词）、ALI\_WS（阿里词典）。 |

**说明：** └表示子参数。

示例如下。

```

[
    {
        "name":"deploy_0.dic",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"MAIN"
    },
    {
        "name":"SYSTEM_MAIN.dic",
        "type":"MAIN",
        "sourceType":"ORIGIN"
    },
    {
        "name":"SYSTEM_STOPWORD.dic",
        "type":"STOP",
        "sourceType":"ORIGIN"
    }
]

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Array of DictList| |返回结果。 |
|fileSize|Long|2782602|文件大小。 |
|name|String|SYSTEM\_MAIN.dic|上传的文件名称。 |
|sourceType|String|ORIGIN|同义词来源类型，支持：OSS（OSS开放存储）、ORIGIN（开源Elasticsearch）、UPLOAD（上传的文件）。 |
|type|String|MAIN|词典类型，支持：STOP（停用词）、MAIN（主词典）、SYNONYMS（同义词）、ALI\_WS（阿里词典）。 |

## 示例

请求示例

```
PUT /openapi/instances/es-cn-nif1q9o8r0008****/dict HTTP/1.1
公共请求头
[
    {
        "name":"deploy_0.dic",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"MAIN"
    },
    {
        "name":"SYSTEM_MAIN.dic",
        "type":"MAIN",
        "sourceType":"ORIGIN"
    },
    {
        "name":"SYSTEM_STOPWORD.dic",
        "type":"STOP",
        "sourceType":"ORIGIN"
    }
]
```

正常返回示例

`XML` 格式

```
<Result>
    <name>deploy_0.dic</name>
    <fileSize>220</fileSize>
    <sourceType>OSS</sourceType>
    <type>MAIN</type>
</Result>
<Result>
    <name>SYSTEM_MAIN.dic</name>
    <fileSize>2782602</fileSize>
    <sourceType>ORIGIN</sourceType>
    <type>MAIN</type>
</Result>
<Result>
    <name>SYSTEM_STOPWORD.dic</name>
    <fileSize>132</fileSize>
    <sourceType>ORIGIN</sourceType>
    <type>STOP</type>
</Result>
<RequestId>671B4AF2-4209-472A-A423-DACF91D9****</RequestId>
```

`JSON` 格式

```
{
	"Result": [
		{
			"name": "deploy_0.dic",
			"fileSize": 220,
			"sourceType": "OSS",
			"type": "MAIN"
		},
		{
			"name": "SYSTEM_MAIN.dic",
			"fileSize": 2782602,
			"sourceType": "ORIGIN",
			"type": "MAIN"
		},
		{
			"name": "SYSTEM_STOPWORD.dic",
			"fileSize": 132,
			"sourceType": "ORIGIN",
			"type": "STOP"
		}
	],
	"RequestId": "671B4AF2-4209-472A-A423-DACF91D9****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

