# ListLogstashLog

调用ListLogstashLog，查看Logstash实例的日志。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListLogstashLog&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId]/search-log HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|beginTime|Long|Query|是|1531910852074|日志开始的时间戳。单位：毫秒。 |
|endTime|Long|Query|是|1531910852074|日志结束的时间戳。单位：毫秒。 |
|InstanceId|String|Path|是|ls-cn-v0h1kzca\*\*\*\*|实例ID。 |
|page|Integer|Query|是|1|实例列表的页码。默认值：1，最小值：1，最大值：200。 |
|query|String|Query|是|host:10.7.xx.xx AND level:info AND content:opening|要查询的关键词。 |
|size|Integer|Query|是|20|分页查询时设置的每页条数。默认值：20，最小值：1，最大值：100。 |
|type|String|Query|是|LOGSTASH\_INSTANCE\_LOG|日志类型。可选值：LOGSTASH\_INSTANCE\_LOG（主日志）、SEARCHSLOW（searching慢日志）、INDEXINGSLOW（indexing慢日志）、JMVLOG（GC日志）、LOGSTASH\_DEBUG\_LOG（调试日志）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|7F40EAA1-6F1D-4DD9-8DB8-C5F00C4E\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|content|String|\[logstash.outputs.fileextend\] Opening file \{:path=\>\\"/ssd/1/ls-cn-v0h1kzca\*\*\*\*/logstash/logs/debug/test\\"\}|日志的详细内容。 |
|host|String|192.168.xx.xx|生成日志的节点的IP地址。 |
|instanceId|String|ls-cn-v0h1kzca\*\*\*\*|实例ID。 |
|level|String|info|日志级别。包括trace、debug、info、warn、error等内容（GC日志没有level）。 |
|timestamp|Long|1531985112420|日志生成的时间戳。单位：毫秒。 |

返回数据中还包含以下参数。

|名称

|类型

|示例值

|描述 |
|----|----|-----|----|
|Result

|Struct

| |返回结果。 |
|└time

|String

|2020-07-22T16:58:00.506Z

|日志产生的时间。 |
|Headers

|Struct

| |返回头信息。 |
|└X-Total-Count

|Integer

|1

|返回的日志数量。 |

**说明：** └表示子参数。

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-v0h1kzca****/search-log?type=LOGSTASH_INSTANCE_LOG&query=host:10.7.xx.xx AND level:info AND content:opening&beginTime=1531910852074&endTime=1531910852074&page=1&size=20 HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"timestamp": 1595408280506,
			"host": "10.7.**.**",
			"contentCollection": {
				"level": "info",
				"host": "10.7.**.**",
				"time": "2020-07-22T16:58:00.506Z",
				"content": "[logstash.outputs.fileextend] Opening file {:path=>\"/ssd/1/ls-cn-v0h1kzca****/logstash/logs/debug/test\"}"
			},
			"instanceId": "ls-cn-v0h1kzca****"
		}
	],
	"RequestId": "DADBEFD2-570D-48EE-ABE4-0E3017D8****",
	"Headers": {
		"X-Total-Count": 1
	}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

