# DescribeXpackMonitorConfig

调用DescribeXpackMonitorConfig，获取Logstash实例的X-Pack监控配置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeXpackMonitorConfig&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/xpack-monitor-config HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-oew1qbgl\*\*\*\*|Logstash实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|enable|Boolean|true|是否开启X-Pack监控：

 -   true：开启
-   false：未开启 |
|endpoints|List|\["http://es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|X-Pack监控关联的Elasticsearch实例的访问地址列表。 |
|esInstanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|X-Pack监控关联的Elasticsearch实例ID。 |
|pipelineIds|List|\[\]|X-Pack监控关联的Kibana实例管理的管道列表。 |
|userName|String|elastic|X-Pack监控关联的Elasticsearch实例的访问用户名。 |

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/xpack-monitor-config HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"esInstanceId": "es-cn-n6w1o1x0w001c****",
		"endpoints": [
			"http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
		],
		"pipelineIds": [],
		"userName": "elastic",
		"enable": true
	},
	"RequestId": "9EC7377A-60D7-4AB2-ADE3-983E1D0D****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

