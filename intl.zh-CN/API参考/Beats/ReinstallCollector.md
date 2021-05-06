# ReinstallCollector

调用ReinstallCollector，重试安装在创建时没有安装成功的采集器。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ReinstallCollector&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/collectors/[ResId]/actions/reinstall HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ResId|String|Path|是|ct-cn-l871nd0u73c45\*\*\*\*|采集器ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定重试安装采集器的机器信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|nodes

|List<String\\\>

|是

|\["ecs-cn-abc"\]

|待重试安装采集器的ECS实例ID。 |
|restartType

|String

|是

|nodeEcsId

|重试安装采集器的类型，目前仅支持nodeEcsId，表示ECS实例。 |

示例如下。

```

{
  "restartType": "nodeEcsId",
  "nodes":["i-bp1gyhphjaj73jsr****","i-bp10piq1mkfnyw9t****"]
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|请求结果：

 -   true：安装成功
-   false：安装失败 |

## 示例

请求示例

```
POST /openapi/collectors/ct-cn-l871nd0u73c45****/actions/reinstall HTTP/1.1
公共请求头
{
  "restartType": "nodeEcsId",
  "nodes": ["i-bp1gyhphjaj73jsr****","i-bp10piq1mkfnyw9t****"]
}
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "F18CF67E-633D-41E8-9172-7DE08052****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

