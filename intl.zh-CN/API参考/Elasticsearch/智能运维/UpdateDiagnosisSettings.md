# UpdateDiagnosisSettings

调用UpdateDiagnosisSettings，更新实例的智能运维场景设置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateDiagnosisSettings&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/diagnosis/instances/[InstanceId]/settings HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-45914gy290009\*\*\*\*|实例ID。 |
|ClientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|lang|String|否|en|返回结果的语言，默认为en。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定智能运维场景的配置。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|scene

|String

|是

|Business Analysis

|待设置的智能运维场景的名称。支持Business Search（业务搜索）、Data Acceleration（数据加速）、Statistics（指标统计）、Business Analysis（业务分析）以及自定义的场景名称。 |

示例如下。

```

{
    "scene":"Business Analysis"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：更新成功
-   false：更新失败 |

## 示例

请求示例

```
PUT /openapi/diagnosis/instances/es-cn-45914gy290009****/settings HTTP/1.1
公共请求头
{ 
   "scene":"Business Analysis"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>5B03F520-E884-4F7B-931D-63766054****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "5B03F520-E884-4F7B-931D-63766054****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

