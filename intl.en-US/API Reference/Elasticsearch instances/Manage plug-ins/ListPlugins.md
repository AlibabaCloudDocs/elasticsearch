# ListPlugins

Call ListPlugins to query plug-ins on a specified Elasticsearch instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListPlugins&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/plugins HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
|name|String|No|analysis-ik|The name of the plug-in. |
|page|String|No|1|The page number of the returned page. |
|size|Integer|No|10|The number of the entries to return on each page. |
|source|String|No|SYSTEM|The source type of the plug-in. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|2|The total number of entries returned. |
|RequestId|String|5A5D8E74-565C-43DC-B031-29289FA9\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|description|String|IK analysis plug-in for Elasticsearch.|The description of the plug-in. |
|name|String|analysis-ik|The name of the plug-in. |
|source|String|SYSTEM|The source type of the plug-in. |
|specificationUrl|String|https://xxxx.html|The address of the plug-in documentation. |
|state|String|INSTALLED|The status of the plug-in. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-nif1q9o8r0008****/plugins HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <name>aliyun-qos</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Rate limiting and throttling plug-in for Elasticsearch. It limits QPS and bulk request sizes and supports rate limiting and throttling for node-level read and write operations.</description>
    <specificationUrl>https://xxxx.html</specificationUrl>
</Result>
<Result>
    <name>aliyun-sql</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>SQL plug-in developed by Alibaba Cloud for Elasticsearch. It provides high SQL query capabilities.</description>
    <specificationUrl>https://xxxx.html</specificationUrl>
</Result>
<Result>
    <name>analysis-aliws</name>
    <state>UNINSTALLED</state>
    <source>SYSTEM</source>
    <description>Aliws analysis plug-in for Elasticsearch. It is integrated with an analyzer and a tokenizer.</description>
    <specificationUrl>https://xxxx.html</specificationUrl>
</Result>
<Result>
    <name>analysis-icu</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>ICU analysis plug-in for Elasticsearch. It integrates the Lucene ICU module into Elasticsearch and adds ICU analysis components.</description>
</Result>
<Result>
    <name>analysis-ik</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>IK analysis plug-in for Elasticsearch.</description>
</Result>
<Result>
    <name>analysis-kuromoji</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Japanese (Kuromoji) analysis plug-in for Elasticsearch. It integrates the Lucene Kuromoji analysis module into Elasticsearch.</description>
</Result>
<Result>
    <name>analysis-phonetic</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Phonetic analysis plug-in for Elasticsearch. It integrates the phonetic token filter into Elasticsearch.</description>
</Result>
<Result>
    <name>analysis-pinyin</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Pinyin analysis plug-in for Elasticsearch.</description>
</Result>
<Result>
    <name>analysis-smartcn</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Smart Chinese analysis plug-in for Elasticsearch. It integrates the Lucene Smart Chinese analysis module into Elasticsearch.</description>
</Result>
<Result>
    <name>apack</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Provides the general physical replication and vector retrieval features. These features improve the write performance of a cluster and enable image search.</description>
    <specificationUrl>https://xxxx.html</specificationUrl>
</Result>
<Result>
    <name>codec-compression</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>The codec-compression plug-in supports lossless compression algorithms such as Brotli and zstd. This plug-in also provides a high index compression ratio and reduces index storage costs.</description>
    <specificationUrl>https://xxxx.html</specificationUrl>
</Result>
<Result>
    <name>elasticsearch-repository-oss</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Alibaba Cloud OSS is supported for storing Elasticsearch snapshots.</description>
</Result>
<Result>
    <name>ingest-attachment</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Ingest processor for Elasticsearch. It uses Apache Tika to extract content.</description>
</Result>
<Result>
    <name>kmonitor</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>aliyun elasticsearch kmonitor plugin</description>
</Result>
<Result>
    <name>mapper-murmur3</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>Computes the hashes of field values when an index is created and stores the hashes in the index.</description>
</Result>
<Result>
    <name>mapper-size</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>The Mapper Size plug-in allows documents to record their uncompressed size at index time.</description>
</Result>
<Result>
    <name>repository-hdfs</name>
    <state>INSTALLED</state>
    <source>SYSTEM</source>
    <description>The HDFS repository plug-in adds support for Hadoop Distributed File System (HDFS) repositories.</description>
</Result>
<RequestId>06280628-C7CB-4818-83EE-8079ACB8****</RequestId>
<Headers>
    <X-Total-Count>17</X-Total-Count>
</Headers>
```

`JSON` format

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

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

