# UpdateDict

Call UpdateDict to cold update the IK word segmentation plug-in of the Alibaba Cloud Elasticsearch instance, including the IK main word segmentation thesaurus and the IK stop word library.

When calling this interface, note:

-   If the dictionary file comes from OSS, make sure that the OSS storage space is publicly readable.
-   If the uploaded dictionary is not configured with ORIGIN, the dictionary file will be deleted after this interface is called.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateDict&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     PUT /openapi/instances/[InstanceId]/dict HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

The following parameters must be filled in the RequestBody.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|name

|String

|Yes

|dic\_0.dic

|The name of the uploaded dictionary file. |
|ossObject

|Array

|Yes

| |The open storage file description of OSS. Required when sourceType is OSS. |
|└bucketName

|String

|Yes

|search-cloud-test-cn-\*\*\*\*

|The name of the OSS bucket. |
|└key

|String

|Yes

|oss/dic\_0.dic

|The path where the dictionary file is stored in OSS Bucket. |
|sourceType

|String

|Yes

|OSS

|The source type of the dictionary file. Optional values: OSS \(OSS Open Storage\) and ORIGIN \(Keep previously uploaded dictionaries\).

**Note:**

Local files need to be uploaded to OSS before being referenced by OSS.

If the dictionary that has been uploaded before is not configured with ORIGIN, it will be deleted by the system. |
|type

|String

|Yes

|MAIN

|The dictionary type to update. Optional values: MAIN\(IK main segmentation thesaurus\) or STOP\(IK deactivation thesaurus\). |

**Note:** └ indicates a child parameter.

The following sample statements are for your reference:

```

     [ { "name":"deploy_0.dic", "ossObject":{ "bucketName":"search-cloud-test-cn-****", "key":"user_dict/dict_0.dic" }, "sourceType":"OSS", "type":"MAIN" }, { "name":"deploy_2.dic", "ossObject":{ "bucketName":"search-cloud-test-cn-****", "key":"user_dict/dict_2.dic" }, "sourceType":"OSS", "type":"STOP" }, { "name":"SYSTEM_MAIN.dic", "sourceType":"ORIGIN", "type":"MAIN" }, { "name":"SYSTEM_STOPWORD.dic", "sourceType":"ORIGIN", "type":"STOP" } ] 
   
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|The ID of the request. |
|Result|Array of DictList| |The return results. |
|fileSize|Long|2782602|The dictionary file size, unit: Byte. |
|name|String|SYSTEM\_MAIN.dic|The dictionary file name. |
|sourceType|String|ORIGIN|Dictionary file source type, supported:

-   OSS:OSS Open Storage
-   ORIGIN: Keep dictionaries that have been uploaded before |
|type|String|MAIN|Dictionary type, supported:

-   MAIN:IK main participle thesaurus
-   STOP:IK stopword library |

## Examples

Sample requests

```

     PUT /openapi/instances/es-cn-oew1q8bev0002 ****/dict HTTP/1.1 public request header [ { "name":"deploy_0.dic", "ossObject":{ "bucketName":"search-cloud-test-cn-****", "key":"user_dict/dict_0.dic" }, "sourceType":"OSS", "type":"MAIN" }, { "name":"deploy_2.dic", "ossObject":{ "bucketName":"search-cloud-test-cn-****", "key":"user_dict/dict_2.dic" }, "sourceType":"OSS", "type":"STOP" }, { "name":"SYSTEM_MAIN.dic", "sourceType":"ORIGIN", "type":"MAIN" }, { "name":"SYSTEM_STOPWORD.dic", "sourceType":"ORIGIN", "type":"STOP" } ] 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "name": "deploy_0.dic", "ossObject": { "bucketName": "search-cloud-test-cn-****", "key": "user_dict/dict_0.dic" }, "sourceType": "OSS", "type": "MAIN" }, { "name": "deploy_2.dic", "ossObject": { "bucketName": "search-cloud-test-cn-****", "key": "user_dict/dict_2.dic" }, "sourceType": "OSS", "type": "STOP" }, { "name": "SYSTEM_MAIN.dic", "sourceType": "ORIGIN", "type": "MAIN" }, { "name": "SYSTEM_STOPWORD.dic", "sourceType": "ORIGIN", "type": "STOP" } ], "RequestId": "E1F6991B-1F77-47EA-9666-593F11E3****" } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

