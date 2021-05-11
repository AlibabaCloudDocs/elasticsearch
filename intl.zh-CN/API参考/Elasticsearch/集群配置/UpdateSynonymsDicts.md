# UpdateSynonymsDicts

调用UpdateSynonymsDicts，更新阿里云Elasticsearch实例的同义词词典。

调用此接口时，请注意：

-   如果词典文件来源于OSS，需要确保OSS存储空间为公共可读。
-   如果已经上传的词典不加ORIGIN配置，调用此接口后，词典文件会被删除。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateSynonymsDicts&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/instances/[InstanceId]/synonymsDict HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

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

|dic\_0.txt

|上传的词典文件名称，必须为TXT类型。 |
|ossObject

|Array

|否

| |OSS的开放存储文件描述。当sourceType为OSS时，必填。 |
|└bucketName

|String

|否

|search-cloud-test-cn-\*\*\*\*

|OSS存储空间（Bucket）名称。 |
|└key

|String

|否

|oss/dic\_0.txt

|词典文件在OSS Bucket中的存储路径。 |
|sourceType

|String

|是

|OSS

|词典文件来源类型，可选值：OSS（使用OSS开放存储）、ORIGIN（保留之前已经上传的词典）。

 **注意：**

 本地文件需要先上传至OSS，再通过OSS引用。

 如果已经完成上传的词典不加ORIGIN进行配置，会被系统删除。 |
|type

|String

|是

|SYNONYMS

|要更新的词典类型，固定为SYNONYMS。 |

**说明：** └表示子参数。

示例如下。

```

[
    {
        "name":"deploy_0.txt",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"SYNONYMS"
    },
    {
        "name":"SYSTEM_STOPWORD.txt",
        "sourceType":"ORIGIN",
        "type":"SYNONYMS"
    }
]


```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|7C5622CC-B312-426F-85AA-B0271\*\*\*\*\*\*\*|请求ID。 |
|Result|Array of DictList| |返回结果。 |
|fileSize|Long|220|词典文件大小，单位：Byte。 |
|name|String|deploy\_0.txt|词典文件名称。 |
|sourceType|String|OSS|词典文件来源类型，支持：

 -   OSS：OSS开放存储
-   ORIGIN：保留之前已经上传的词典 |
|type|String|SYNONYMS|词典类型，支持：SYNONYMS（同义词）。 |

## 示例

请求示例

```
PUT /openapi/instances/es-cn-nif1q9o8r0008****/synonymsDict HTTP/1.1
公共请求头
[
    {
        "name":"deploy_0.txt",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"SYNONYMS"
    },
    {
        "name":"SYSTEM_STOPWORD.txt",
        "sourceType":"ORIGIN",
        "type":"SYNONYMS"
    }
]
```

正常返回示例

`JSON`格式

```
{
    "Result": [

        {
            "name":"deploy_0.txt",
            "fileSize":220,
            "sourceType":"OSS",
            "type":"SYNONYMS"
        },
        {
            "name":"SYSTEM_MAIN.txt",
            "fileSize":2782602,
            "sourceType":"ORIGIN",
            "type":"SYNONYMS"
        },
        {
            "name":"SYSTEM_STOPWORD.txt",
            "fileSize":132,
            "sourceType":"ORIGIN",
            "type":"SYNONYMS"
        }
    ],
    "RequestId": "1F7FE662-CCD8-474F-BA9B-A7E0792E****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

