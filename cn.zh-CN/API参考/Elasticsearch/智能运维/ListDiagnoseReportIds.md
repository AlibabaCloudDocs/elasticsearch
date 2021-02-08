# ListDiagnoseReportIds

调用ListDiagnoseReportIds，获取智能运维历史报告的ID。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDiagnoseReportIds&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/diagnosis/instances/[InstanceId]/report-ids HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|endTime|Long|Query|是|1595174399999|查询结束时间戳。 |
|InstanceId|String|Path|是|es-cn-n6w1qu7ei000p\*\*\*\*|实例ID。 |
|startTime|Long|Query|是|1595088000000|查询开始时间戳。 |
|lang|String|Query|否|spanish|获取的报告的语言。 |
|page|Integer|Query|否|1|分页数。默认值：1，最小值：1，最大值：200。 |
|size|Integer|Query|否|15|每页报告ID的数量。默认值：10，最小值：1，最大值：500。 |
|trigger|String|Query|否|SYSTEM|健康诊断的触发方式，支持：SYSTEM（系统自动触发）、INNER（内部触发）和USER（用户手动触发）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|1|返回的总记录数。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|List|\["scheduled\_\_2020-09-13T00:40:00"\]|返回结果。 |

## 示例

请求示例

```
GET /openapi/diagnosis/instances/es-cn-09k1rocex0006****/report-ids?startTime=1600099200000&endTime=1600185600000 HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>scheduled__2020-09-15T00:40:00</Result>
<RequestId>B5F822C5-03E9-4899-8D80-2B35515A****</RequestId>
<Headers>
    <X-Total-Count>1</X-Total-Count>
</Headers>
```

`JSON`格式

```
{
	"Result": [
		"scheduled__2020-09-15T00:40:00"
	],
	"RequestId": "B5F822C5-03E9-4899-8D80-2B35515A****",
	"Headers": {
		"X-Total-Count": 1
	}
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

