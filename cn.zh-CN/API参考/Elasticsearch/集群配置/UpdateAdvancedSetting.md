# UpdateAdvancedSetting

调用UpdateAdvancedSetting，更改指定实例的垃圾回收器配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateAdvancedSetting&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST|PUT /openapi/instances/[InstanceId]/actions/update-advanced-setting HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-09k1ruw79000u\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定修改后的垃圾回收器配置。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|gcName

|String

|是

|CMS

|修改后的垃圾回收器名称。支持：CMS、G1。 |

示例如下。

```

{
    "gcName":"CMS"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：垃圾回收器配置更改成功
-   false：垃圾回收器配置更改失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-09k1ruw79000u****/actions/update-advanced-setting HTTP/1.1
公共请求头
{
    "gcName":"CMS"
}
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "D7BA7E23-F6B7-4D57-BBE4-67EACAAB****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

