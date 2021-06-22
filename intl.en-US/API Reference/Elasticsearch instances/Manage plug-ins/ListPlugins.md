# ListPlugins

You can call this operation to query plug-ins on a specified Elasticsearch instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListPlugins&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters. For more information, see the Common request parameters topic.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/plugins HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Location|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The IDs of the added ECS instances. |
|name|String|Query|No|analysis-ik|The name of the plug-in. |
|page|String|Query|No|1|The number of the returned page. |
|size|Integer|Query|No|10|The number of entries returned on each page. |
|source|String|Query|No|SYSTEM|The plug-in source type. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|2|The total number of entries returned. |
|RequestId|String|5A5D8E74-565C-43DC-B031-29289FA9\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The returned results. |
|description|String|IK analysis plug-in for Elasticsearch.|The description of the plug-in. |
|name|String|analysis-ik|The name of the plug-in. |
|source|String|SYSTEM|The plug-in source type. |
|specificationUrl|String|https://xxxx.html|The address of the plug-in description document. |
|state|String|INSTALLED|The status of the plug-in. |

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-nif1q9o8r0008 ****/plugins HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "name": "aliyun-qos", "state": "INSTALLED", "source": "SYSTEM", "description": "Rate limiting and throttling plug-in for Elasticsearch. It limits QPS and bulk request sizes and supports rate limiting and throttling for node-level read and write operations.", "specificationUrl": "https://xxxx.html" }, { "name": "aliyun-sql", "state": "INSTALLED", "source": "SYSTEM", "description": "SQL plug-in developed by Alibaba Cloud for Elasticsearch. It provides high SQL query capabilities.", "specificationUrl": "https://xxxx.html" }, { "name": "analysis-aliws", "state": "UNINSTALLED", "source": "SYSTEM", "description": "Aliws analysis plug-in for Elasticsearch. It is integrated with an analyzer and a tokenizer.", "specificationUrl": "https://xxxx.html" }, { "name": "analysis-icu", "state": "INSTALLED", "source": "SYSTEM", "description": "ICU analysis plug-in for Elasticsearch. It integrates the Lucene ICU module into Elasticsearch and adds ICU analysis components." }, { "name": "analysis-ik", "state": "INSTALLED", "source": "SYSTEM", "description": "IK analysis plug-in for Elasticsearch." }, { "name": "analysis-kuromoji", "state": "INSTALLED", "source": "SYSTEM", "description": "Japanese (Kuromoji) analysis plug-in for Elasticsearch. It integrates the Lucene Kuromoji analysis module into Elasticsearch." }, { "name": "analysis-phonetic", "state": "INSTALLED", "source": "SYSTEM", "description": "Phonetic analysis plug-in for Elasticsearch. It integrates the phonetic token filter into Elasticsearch." }, { "name": "analysis-pinyin", "state": "INSTALLED", "source": "SYSTEM", "description": "Pinyin analysis plug-in for Elasticsearch." }, { "name": "analysis-smartcn", "state": "INSTALLED", "source": "SYSTEM", "description": "Smart Chinese analysis plug-in for Elasticsearch. It integrates the Lucene Smart Chinese analysis module into Elasticsearch." }, { "name": "apack", "state": "INSTALLED", "source": "SYSTEM", "description": "Provides the general physical replication and vector retrieval features. These features improve the write performance of a cluster and enable image search.", "specificationUrl": "https://xxxx.html" }, { "name": "codec-compression", "state": "INSTALLED", "source": "SYSTEM", "description": "The codec-compression plug-in supports lossless compression algorithms such as Brotli and zstd. This plug-in also provides a high index compression ratio and reduces index storage costs.", "specificationUrl": "https://xxxx.html" }, { "name": "elasticsearch-repository-oss", "state": "INSTALLED", "source": "SYSTEM", "description": "Alibaba Cloud OSS is supported for storing Elasticsearch snapshots." }, { "name": "ingest-attachment", "state": "INSTALLED", "source": "SYSTEM", "description": "Ingest processor for Elasticsearch. It uses Apache Tika to extract content." }, { "name": "kmonitor", "state": "INSTALLED", "source": "SYSTEM", "description": "aliyun elasticsearch kmonitor plugin" }, { "name": "mapper-murmur3", "state": "INSTALLED", "source": "SYSTEM", "description": "Computes the hashes of field values when an index is created and stores the hashes in the index." }, { "name": "mapper-size", "state": "INSTALLED", "source": "SYSTEM", "description": "The Mapper Size plug-in allows documents to record their uncompressed size at index time." }, { "name": "repository-hdfs", "state": "INSTALLED", "source": "SYSTEM", "description": "The HDFS repository plug-in adds support for Hadoop Distributed File System (HDFS) repositories." } ], "RequestId": "06280628-C7CB-4818-83EE-8079ACB8****", "Headers": { "X-Total-Count": 17 } } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Please check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

