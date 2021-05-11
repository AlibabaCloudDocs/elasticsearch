# EstimatedRestartTime

调用EstimatedRestartTime，获取重启实例的预估时间。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=EstimatedRestartTime&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/estimated-time/restart-time HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|force|Boolean|Query|否|false|是否是强制重启。默认：false。 |

## RequestBody

RequestBody中还可以填入以下参数，用来指定重启参数信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|restartType

|String

|是

|instance

|重启类型，支持：instance（重启实例，默认）、nodeIp（节点重启）。 |
|nodes

|List<String\\\>

|否

|\["127.0.0.1"\]

|选择节点重启时，目标节点的IP地址列表。 |
|blueGreenDep

|Boolean

|否

|false

|节点重启时，是否进行蓝绿变更，默认为false。 |
|batch

|Integer

|否

|25.0

|实例强制重启的并发度。默认：1/实例总节点数。 |
|batchUnit

|String

|否

|percent

|batch单位。默认：percent。 |

-   restartType为instance时，忽略blueGreenDep参数。
    -   force为true，batch必须大于0，小于等于100，否则系统会提示RestartBatchValueError的报错。
    -   force为false，batch默认为0，输入其他值时，会报错NormalRestartNotSupportBatch。

-   restartType为nodeIp时，忽略batch参数。
    -   nodeIp为空，系统会提示参数错误。
    -   blueGreenDep为true，进行蓝绿变更重启；为false，正常重启。

示例如下。

```

{
    "restartType":"nodeIp",
    "nodes": ["172.16.xx.xx"],
    "blueGreenDep":true
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|unit|String|second|单位。 |
|value|Long|50|重启预估时间。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/estimated-time/restart-time?force=true HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"unit": "second",
		"value": 4200
	},
	"RequestId": "7ACE8751-DD1B-40DB-A253-9080CA58****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

