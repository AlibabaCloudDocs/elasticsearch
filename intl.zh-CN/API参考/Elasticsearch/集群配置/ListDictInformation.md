# ListDictInformation

调用ListDictInformation，在添加用户OSS存储的词典文件时，获取和校验用户OSS词典文件的详情。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDictInformation&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/dict/_info HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|bucketName|String|Query|是|search-cloud-test-cn-\*\*\*\*|词典文件所在的OSS存储空间（Bucket）名称。 |
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|key|String|Query|是|oss/dic\_0.dic|词典文件在OSS Bucket中的存储路径。 |
|analyzerType|String|Query|否|ALIWS|用户待添加的OSS词典类型。支持IK\_HOT、IK、SYNONYMS、ALIWS四种类型。默认值：IK |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|7C4334EA-D22B-48BD-AE28-08EE68\*\*\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|fileSize|Long|2202301|词典文件大小，单位：Byte。 |
|ossObject|Struct| |OSS开放存储文件详情。 |
|bucketName|String|es-osstest\*|OSS存储文件所在的空间（Bucket）名称。 |
|etag|String|2ABAB5E70BBF631145647F6BE533\*\*\*\*|OSS存储文件的MD5校验码Etag（大写）。 |
|key|String|oss/dict\_0\*.dic|词典文件在OSS Bucket中的存储路径。 |
|type|String|STOP|词库类型，支持以下两种类型：

 -   MAIN：主分词词库
-   STOP：停用词库 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/dict/_info?bucketName=your-oss-bucket-name&key=test/dict.dic&analyzerType=ALIWS 
HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "7C4334EA-D22B-48BD-AE28-08EE68******",
    "Result": {
        "fileSize": 2202301,
        "type": "STOP",
        "ossObject": {
            "bucketName": "es-osstest*",
            "etag": "2ABAB5E70BBF631145647F6BE533****",
            "key": "oss/dict_0*.dic"
        }
    }
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

