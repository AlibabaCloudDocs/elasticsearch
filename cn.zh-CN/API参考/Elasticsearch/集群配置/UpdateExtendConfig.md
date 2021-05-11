# UpdateExtendConfig

调用UpdateExtendConfig，修改集群的场景化配置模板。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateExtendConfig&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/extend-configs/actions/update HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定场景化配置模板信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|configType

|String

|是

|usageScenario

|配置类型。支持：usageScenario（使用场景化配置）。 |
|value

|String

|是

|general

|商业版实例可选：general（通用场景）、analysisVisualization（数据分析场景）、dbAcceleration（数据库加速场景）、search（搜索场景）。

 增强版实例可选：log（日志场景）。 |

示例如下。

```

{
    "configType": "usageScenario",
    "value": "search"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：修改场景模板配置成功
-   false：修改场景模板配置失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/extend-configs/actions/update?ClientToken=5A2CFF0E-5718-45B5-9D4D-70B3FF**** HTTP/1.1
公共请求头
{
    "configType":"usageScenario",
    "value":"general"
}
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "97299FD7-FD89-455E-BC35-3AB26027****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

