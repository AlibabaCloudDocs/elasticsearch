# UpdateExtendfiles

调用UpdateExtendfiles，更新Logstash实例的扩展文件配置。

调用此接口时，请注意：

目前此接口仅支持删除控制台已上传的Logstash扩展文件。如需添加或修改，可在控制台上操作。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateExtendfiles&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/logstashes/[InstanceId]/extendfiles HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定更新后的扩展文件配置。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|name

|String

|是

|mysql-connector-java-6.0.2.jar

|扩展文件名称。文件后缀必须是.jar，文件名不支持中文，且长度不超过100个字符。 |
|sourceType

|String

|是

|ORIGIN

|扩展文件来源，目前只支持ORIGIN。即保留对应扩展文件，未配置该参数的扩展文件会被删除。添加和修改扩展文件功能正在开发中，您可以在控制实现全部管控操作。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|fileSize|Long|1853083|扩展文件大小，单位：Byte。 |
|name|String|mysql-connector-java-6.0.2.jar|扩展文件名称。 |
|sourceType|String|ORIGIN|扩展文件来源，仅支持ORIGIN（保留的原有扩展文件）。 |

## 示例

请求示例

```
PUT /openapi/logstashes/ls-cn-oew1qbgl****/extendfiles HTTP/1.1
公共请求头
[
    {
        "sourceType":"ORIGIN",
        "name":"mysql-connector-java-5.1.48.jar"
    }
]
```

正常返回示例

`XML`格式

```
<Result>
    <name>mysql-connector-java-5.1.35.jar</name>
    <fileSize>968668</fileSize>
    <sourceType>ORIGIN</sourceType>
</Result>
<RequestId>27F32ECF-0527-43BF-A116-D6260D1240BE</RequestId>
```

`JSON`格式

```
{
  "Result": [
    {
      "name": "mysql-connector-java-5.1.35.jar",
      "fileSize": 968668,
      "sourceType": "ORIGIN"
    }
  ],
  "RequestId": "27F32ECF-0527-43BF-A116-D6260D1240BE"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

