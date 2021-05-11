# ListPlugins

调用ListPlugins，获取指定阿里云Elasticsearch实例的插件列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListPlugins&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/plugins HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|name|String|Query|否|analysis-ik|插件名称。 |
|page|String|Query|否|1|分页数。 |
|size|Integer|Query|否|10|每页记录数。 |
|source|String|Query|否|SYSTEM|插件来源类型。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|2|返回的总记录数。 |
|RequestId|String|5A5D8E74-565C-43DC-B031-29289FA9\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|description|String|IK analysis plug-in for Elasticsearch.|插件描述。 |
|name|String|analysis-ik|插件名称。 |
|source|String|SYSTEM|插件来源类型。 |
|specificationUrl|String|https://xxxx.html|插件说明文档的地址。 |
|state|String|INSTALLED|插件状态。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-nif1q9o8r0008****/plugins HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"name": "aliyun-qos",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Rate limiting and throttling plug-in for Elasticsearch. It limits QPS and bulk request sizes and supports rate limiting and throttling for node-level read and write operations.",
			"specificationUrl": "https://xxxx.html"
		},
		{
			"name": "aliyun-sql",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "SQL plug-in developed by Alibaba Cloud for Elasticsearch. It provides high SQL query capabilities.",
			"specificationUrl": "https://xxxx.html"
		},
		{
			"name": "analysis-aliws",
			"state": "UNINSTALLED",
			"source": "SYSTEM",
			"description": "Aliws analysis plug-in for Elasticsearch. It is integrated with an analyzer and a tokenizer.",
			"specificationUrl": "https://xxxx.html"
		},
		{
			"name": "analysis-icu",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "ICU analysis plug-in for Elasticsearch. It integrates the Lucene ICU module into Elasticsearch and adds ICU analysis components."
		},
		{
			"name": "analysis-ik",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "IK analysis plug-in for Elasticsearch."
		},
		{
			"name": "analysis-kuromoji",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Japanese (Kuromoji) analysis plug-in for Elasticsearch. It integrates the Lucene Kuromoji analysis module into Elasticsearch."
		},
		{
			"name": "analysis-phonetic",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Phonetic analysis plug-in for Elasticsearch. It integrates the phonetic token filter into Elasticsearch."
		},
		{
			"name": "analysis-pinyin",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Pinyin analysis plug-in for Elasticsearch."
		},
		{
			"name": "analysis-smartcn",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Smart Chinese analysis plug-in for Elasticsearch. It integrates the Lucene Smart Chinese analysis module into Elasticsearch."
		},
		{
			"name": "apack",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Provides the general physical replication and vector retrieval features. These features improve the write performance of a cluster and enable image search.",
			"specificationUrl": "https://xxxx.html"
		},
		{
			"name": "codec-compression",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "The codec-compression plug-in supports lossless compression algorithms such as Brotli and zstd. This plug-in also provides a high index compression ratio and reduces index storage costs.",
			"specificationUrl": "https://xxxx.html"
		},
		{
			"name": "elasticsearch-repository-oss",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Alibaba Cloud OSS is supported for storing Elasticsearch snapshots."
		},
		{
			"name": "ingest-attachment",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Ingest processor for Elasticsearch. It uses Apache Tika to extract content."
		},
		{
			"name": "kmonitor",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "aliyun elasticsearch kmonitor plugin"
		},
		{
			"name": "mapper-murmur3",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "Computes the hashes of field values when an index is created and stores the hashes in the index."
		},
		{
			"name": "mapper-size",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "The Mapper Size plug-in allows documents to record their uncompressed size at index time."
		},
		{
			"name": "repository-hdfs",
			"state": "INSTALLED",
			"source": "SYSTEM",
			"description": "The HDFS repository plug-in adds support for Hadoop Distributed File System (HDFS) repositories."
		}
	],
	"RequestId": "06280628-C7CB-4818-83EE-8079ACB8****",
	"Headers": {
		"X-Total-Count": 17
	}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

