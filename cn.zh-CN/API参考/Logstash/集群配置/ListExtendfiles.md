# ListExtendfiles

调用ListExtendfiles，获取Logstash实例的扩展文件配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListExtendfiles&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/extendfiles HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|filePath|String|/ssd/1/share/ls-cn-oew1qbgl\*\*\*\*/logstash/current/config/custom/mysql-connector-java-5.1.35.jar|扩展文件路径。 |
|fileSize|Long|968668|扩展文件大小。 |
|name|String|mysql-connector-java-5.1.35.jar|扩展文件名称。 |
|sourceType|String|ORIGIN|来源类型。 |

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/extendfiles HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"name": "mysql-connector-java-5.1.35.jar",
			"fileSize": 968668,
			"sourceType": "ORIGIN",
			"filePath": "/ssd/1/share/ls-cn-oew1qbgl****/logstash/current/config/custom/mysql-connector-java-5.1.35.jar"
		}
	],
	"RequestId": "741099F7-F490-4679-A52E-38601EE7****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

