# ListDiagnoseIndices

调用ListDiagnoseIndices，获取指定实例智能运维模块中，健康诊断的诊断索引。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDiagnoseIndices&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/diagnosis/instances/[InstanceId]/indices HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|lang|String|Query|否|en|语言配置，支持多种语言。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F05ED12E-140A-4ACB-B059-3A508A69F2E1|请求ID。 |
|Result|List|\["my\_index\_aliws", "aliyun-index-test","filebeat-6.7.0-2020.11.15", "filebeat-6.7.0-2020.12.27"\]|诊断索引列表。 |

## 示例

请求示例

```
GET /openapi/diagnosis/instances/es-cn-n6w1o1x0w001c****/indices HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>my_index_aliws</Result>
<Result>aliyun-index-test</Result>
<Result>filebeat-6.7.0-2020.11.15</Result>
<Result>filebeat-6.7.0-2020.12.27</Result>
<RequestId>F05ED12E-140A-4ACB-B059-3A508A69F2E1</RequestId>
```

`JSON`格式

```
{
	"Result": [
		"my_index_aliws",
		"aliyun-index-test",
		"filebeat-6.7.0-2020.11.15",
		"filebeat-6.7.0-2020.12.27"
	],
	"RequestId": "F05ED12E-140A-4ACB-B059-3A508A69F2E1"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

