# DescribeDiagnosisSettings

调用DescribeDiagnosisSettings，获取智能运维的场景设置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeDiagnosisSettings&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/diagnosis/instances/[InstanceId]/settings HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-68n1n8b7f000a\*\*\*\*|实例ID。 |
|lang|String|否|en|设置返回结果的语言，默认为en。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5E82B8A8-EED7-4557-A6E9-D1AD3E58\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|scene|String|Business Search|智能运维的应用场景。 |
|updateTime|Long|1588994035385|上次更新智能运维应用场景的时间戳。 |

## 示例

请求示例

```
GET /openapi/diagnosis/instances/es-cn-45914gy290009****/settings HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <scene>Business Search</scene>
    <updateTime>1561969800000</updateTime>
</Result>
<RequestId>592414F7-652B-4360-91AB-85C53B32****</RequestId>
```

`JSON` 格式

```
{
	"Result": {
		"scene": "Business Search",
		"updateTime": 1561969800000
	},
	"RequestId": "592414F7-652B-4360-91AB-85C53B32****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

