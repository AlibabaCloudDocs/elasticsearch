# ListDicts

调用createInstance，用于返回指定类型的词典详情以及签名生成的公网可下载链接。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDicts&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/dicts HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|analyzerType|String|Query|是|IK|词典类型，支持：

 -   IK：IK冷更新词典。
-   IK\_HOT：IK热更新词典。
-   SYNONYMS：同义词。
-   ALIWS：阿里词典。 |
|InstanceId|String|Path|是|es-cn-0ju29ifnc0005\*\*\*\*|实例ID。 |
|name|String|Query|否|SYSTEM\_MAIN.dic|筛选指定的文件名称。 |

**说明：** 每种elasticsearchAnalyzer下的词典name是唯一的，即使IK的主分词库和停用分词库也不会存在相同name的词典文件。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|1|总记录数。 |
|RequestId|String|2937F832-F39E-41EF-89BA-B528342A2A3A|请求ID。 |
|Result|Array of Result| |返回的请求结果。 |
|downloadUrl|String|http://test\_bucket.oss-cn-hangzhou.aliyuncs.com/AliyunEs/test.dic?Expires=162573\*\*\*\*&OSSAccessKeyId=LTAI\*\*\*\*\*V9&Signature=PNPO\*\*\*\*\*\*\*\*BBGsJDO4V3VfU4sE%3D|公网可下载链接。有效时长为90秒。 |
|fileSize|Long|2782602|词典文件的字节数大小，单位：Byte。 |
|name|String|SYSTEM\_MAIN.dic|词典文件的文件名。 |
|sourceType|String|ORIGIN|固定值。 |
|type|String|MAIN|IK词典的类型，取值含义如下：

 -   MAIN：主分词词库。
-   STOP：停用词词库。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-0ju29ifnc0005****/dicts HTTP/1.1
公共请求头
{
"elasticsearchAnalyzer": "IK"
}
```

正常返回示例

`JSON`格式

```
{
    "Result": [
        {
            "name": "SYSTEM_MAIN.dic",
            "fileSize": 2782602,
            "downloadUrl": "http://test_bucket.oss-cn-hangzhou.aliyuncs.com/AliyunEs/test.dic?Expires=162573****&OSSAccessKeyId=LTAI*****V9&Signature=PNPO********BBGsJDO4V3VfU4sE%3D",
            "sourceType": "ORIGIN",
            "type": "MAIN"
        }
    ],
    "RequestId": "2937F832-F39E-41EF-89BA-B528342A2A3A",
    "Headers": {
        "X-Total-Count": 1
    }
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

