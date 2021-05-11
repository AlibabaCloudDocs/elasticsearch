# UpdateAliwsDict

Call the UpdateAliwsDict to update the dictionary file of the AliNLP word Breaker \(analysis-aliws\). Supports custom dictionary configuration.

Note the following when calling this interface:

-   Alibaba Cloud Elasticsearch V5.0 clusters do not support the analysis-aliws plug-in.
-   If the dictionary file is obtained from OSS, make sure that the OSS bucket is public-readable.
-   If the ORIGIN configuration is not added to an uploaded dictionary file, the dictionary file is deleted after you call this operation.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateAliwsDict&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters, and does not involve special request headers. For more information, see the topic about common parameters.

## Request syntax

```
PUT /openapi/instances/[InstanceId]/aliws-dict HTTP/1.1
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

Enter the following parameters in RequestBody.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|name

|String

|Yes

|aliws\_ext\_dict.txt

|The name of the uploaded dictionary file. |
|ossObject

|Array

|Yes

| |The description of the open storage file of OSS. When sourceType is set to OSS, this parameter is required. |
|└bucketName

|String

|Yes

|search-cloud-test-cn-\*\*\*\*

|The name of an OSS Bucket. |
|└key

|String

|Yes

|oss/aliws\_ext\_dict.txt

|The storage path of the dictionary file in the OSS Bucket. |
|sourceType

|String

|Yes

|OSS

|The type of the Dictionary File Source. Valid values: OSS \(open storage service using OSS\) and ORIGIN \(retaining the dictionaries that have been previously uploaded\).

**Note:**

The local file must be uploaded to OSS and then referenced through OSS.

If a dictionary is not configured with an ORIGIN in the previous upload, it is deleted by the system. |
|type

|String

|Yes

|ALI\_WS

|The type of the dictionary. Static field: ALI\_WS\(AliNLP word segmentation\). |

**Note:** └ indicates a child parameter.

Examples:

```
[
    {
        "name":"deploy_0.txt",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.txt"
        },
        "sourceType":"OSS",
        "type":"ALI_WS"
    },
    {
        "name":"aliws_ext_dict.txt",
        "sourceType":"ORIGIN",
       "type":"ALI_WS"
    }
]
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of DictList| |The return results. |
|fileSize|Long|6226|The size of the file. Unit: Byte. |
|name|String|aliws\_ext\_dict.txt|The name of the uploaded file. |
|sourceType|String|OSS|The source type of the Dictionary File. Valid values:

-   OSS: Use OSS open storage.
-   ORIGIN: Retains dictionaries that have been uploaded. |
|type|String|ALI\_WS|The dictionary type. Valid values: ALI\_WS\(AliNLP word segmentation\). |

## Examples

Sample requests

```
PUT /openapi/instances/es-cn-n6w1o1x0w001c****/aliws-dict HTTP/1.1
common request header
[
    {
        "name":"deploy_0.txt",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.txt"
        },
        "sourceType":"OSS",
        "type":"ALI_WS"
    },
    {
        "name":"aliws_ext_dict.txt",
        "sourceType":"ORIGIN",
       "type":"ALI_WS"
    }
]
```

Sample success responses

`XML` format

```
<Result>
    <name>aliws_ext_dict.txt</name>
    <fileSize>6243</fileSize>
    <sourceType>OSS</sourceType>
    <type>ALI_WS</type>
</Result>
<RequestId>6A185DDB-3E87-448B-8932-8F77E35****</RequestId>
```

`JSON` format

```
{
    "Result":[
        {
            "name":"aliws_ext_dict.txt",
            "fileSize":6243,
            "sourceType":"OSS",
            "type":"ALI_WS"
        }
    ],
    "RequestId":"6A185DDB-3E87-448B-8932-8F77E35****"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

