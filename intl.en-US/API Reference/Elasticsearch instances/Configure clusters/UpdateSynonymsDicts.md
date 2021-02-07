# UpdateSynonymsDicts

Call the UpdateSynonymsDicts operation to update the synonym dictionary of an Alibaba Cloud Elasticsearch instance.

Note the following when calling this interface:

-   If the dictionary file is obtained from OSS, make sure that the OSS bucket is public-readable.
-   If the ORIGIN configuration is not added to an uploaded dictionary file, the dictionary file is deleted after you call this operation.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateSynonymsDicts&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
PUT /openapi/instances/[InstanceId]/synonymsDict HTTP/1.1 
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
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

|dic\_0.txt

|The name of the uploaded Dictionary File. The file must be in the TXT type. |
|ossObject

|Array

|No

| |The description of the open storage file of OSS. When sourceType is set to OSS, this parameter is required. |
|└bucketName

|String

|No

|search-cloud-test-cn-\*\*\*\*

|The name of an OSS Bucket. |
|└key

|String

|No

|oss/dic\_0.txt

|The storage path of the dictionary file in the OSS Bucket. |
|sourceType

|String

|Yes

|OSS

|The type of the Dictionary File Source. Valid values: OSS \(open storage service using OSS\) and ORIGIN \(retaining the dictionaries that have been previously uploaded\).

**Note:**

The local file must be uploaded to OSS and then referenced through OSS.

If an uploaded dictionary is not configured with an ORIGIN, it is deleted by the system. |
|type

|String

|Yes

|SYNONYMS

|The type of the dictionary that you want to update. The value is fixed to SYNONYMS. |

**Note:** └ indicates a child parameter.

Example:

```
[
    {
        "name":"deploy_0.txt",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"SYNONYMS"
    },
    {
        "name":"SYSTEM_STOPWORD.txt",
        "sourceType":"ORIGIN",
        "type":"SYNONYMS"
    }
]
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|7C5622CC-B312-426F-85AA-B0271\*\*\*\*\*\*\*|The ID of the request. |
|Result|Array of DictList| |The return results. |
|fileSize|Long|220|The size of the Dictionary File. Unit: Byte. |
|name|String|deploy\_0.txt|The name of the dictionary file. |
|sourceType|String|OSS|The source type of the Dictionary File. Valid values:

-   OSS: OSS open storage.
-   ORIGIN: Retains dictionaries that have been uploaded. |
|type|String|SYNONYMS|The dictionary type. Valid values: SYNONYMS. |

## Examples

Sample requests

```
PUT /openapi/instances/es-cn-nif1q9o8r0008 **** /synonymdict HTTP/1.1 
common request header
[
    {
        "name":"deploy_0.txt",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"SYNONYMS"
    },
    {
        "name":"SYSTEM_STOPWORD.txt",
        "sourceType":"ORIGIN",
        "type":"SYNONYMS"
    }
]  
```

Sample success responses

`XML` format

```
<Result>
    <name>deploy_0.txt</name>
    <fileSize>220</fileSize>
    <sourceType>OSS</sourceType>
    <type>SYNONYMS</type>
</Result>
<Result>
    <name>SYSTEM_MAIN.txt</name>
    <fileSize>2782602</fileSize>
    <sourceType>ORIGIN</sourceType>
    <type>SYNONYMS</type>
</Result>
<Result>
    <name>SYSTEM_STOPWORD.txt</name>
    <fileSize>132</fileSize>
    <sourceType>ORIGIN</sourceType>
    <type>SYNONYMS</type>
</Result>
<RequestId>1F7FE662-CCD8-474F-BA9B-A7E0792E****</RequestId>
```

`JSON` Syntax

```
{
    "Result": [

        {
            "name":"deploy_0.txt",
            "fileSize":220,
            "sourceType":"OSS",
            "type":"SYNONYMS"
        },
        {
            "name":"SYSTEM_MAIN.txt",
            "fileSize":2782602,
            "sourceType":"ORIGIN",
            "type":"SYNONYMS"
        },
        {
            "name":"SYSTEM_STOPWORD.txt",
            "fileSize":132,
            "sourceType":"ORIGIN",
            "type":"SYNONYMS"
        }
    ],
    "RequestId": "1F7FE662-CCD8-474F-BA9B-A7E0792E****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

