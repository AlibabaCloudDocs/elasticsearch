# UpdateReadWritePolicy

调用UpdateReadWritePolicy，开启或关闭集群的写入高可用特性。目前仅支持华北2（北京）区域的实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateReadWritePolicy&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/update-read-write-policy HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-oew1oxiro000f\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定高可用特性的参数配置。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|writeHa

|Boolean

|否

|true

|是否开启写入高可用特性。true表示是，false表示否。 |
|autoGeneratePk

|Boolean

|否

|true

|无主键时，是否自动生成文档哈希值主键。true为默认值，表示自动生成主键；false表示不会自动生成主键。

 autoGeneratePk不可单独修改，只有在writeHa从false更新为true的时候，同时设置autoGeneratePk才生效。 |
|writePolicy

|String

|否

|sync

|设置临时切换同步和异步高可用。只有在开通高可用，即writeHa为true的情况下，设置此字段才有效。设置此字段时不需要同时传入writeHa字段。

 可选值：

 sync：临时由异步写入高可用切换为同步。

 async：临时开启同步写入后，恢复异步写入高可用。 |

示例如下。

```

{
    "writeHa":true,
    "autoGeneratePk":true
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：开启或关闭写入高可用成功
-   false：开启或关闭写入高可用失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-oew1oxiro000f****/actions/update-read-write-policy HTTP/1.1
公共请求头
{
    "writeHa":true,
    "autoGeneratePk":true
}
```

正常返回示例

`JSON`格式

```
{
	"RequestId": "E29B6B26-1040-4829-972D-3D6459A5****",
	"Result": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

