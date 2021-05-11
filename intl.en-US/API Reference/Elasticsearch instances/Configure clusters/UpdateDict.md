# UpdateDict

Call the UpdateDict to cold update the IK word segmentation plug-ins of a Alibaba Cloud Elasticsearch instance, including the IK main word segmentation dictionary and IK stopword Dictionary.

Note the following when calling this interface:

-   If the dictionary file is obtained from OSS, make sure that the OSS bucket is public-readable.
-   If the ORIGIN configuration is not added to an uploaded dictionary file, the dictionary file is deleted after you call this operation.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateDict&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

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

|dic\_0.dic

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

|oss/dic\_0.dic

|The storage path of the dictionary file in the OSS Bucket. |
|sourceType

|String

|Yes

|OSS

|The type of the Dictionary File Source. Valid values: OSS \(open storage service using OSS\) and ORIGIN \(retaining the dictionaries that have been previously uploaded\).

**Note:**

The local file must be uploaded to OSS and then referenced through OSS.

If you do not add an ORIGIN to the dictionary file, it is deleted by the system. |
|type

|String

|Yes

|MAIN

|The type of the dictionary that you want to update. Valid values: MAIN\(IK MAIN and word dictionaries\) or STOP\(IK disabled dictionaries\). |

**Note:** └ indicates a child parameter.

Example:

```
[
    {
        "name":"deploy_0.dic",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"MAIN"
    },
    {
        "name":"deploy_2.dic",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_2.dic"
        },
        "sourceType":"OSS",
        "type":"STOP"
    },
    {
        "name":"SYSTEM_MAIN.dic",
        "sourceType":"ORIGIN",
         "type":"MAIN"
    },
    {
        "name":"SYSTEM_STOPWORD.dic",
        "sourceType":"ORIGIN",
       "type":"STOP"
    }
]
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|The ID of the request. |
|Result|Array of DictList| |The return results. |
|fileSize|Long|2782602|The size of the Dictionary File. Unit: Byte. |
|name|String|SYSTEM\_MAIN.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source type of the Dictionary File. Valid values:

-   OSS: OSS open storage.
-   ORIGIN: Retains dictionaries that have been uploaded. |
|type|String|MAIN|The type of the dictionary. Valid values:

-   MAIN: IK MAIN word dictionary
-   STOP: IK stopword Dictionary |

## Examples

Sample requests

```
PUT /openapi/instances/es-cn-oew1q8bev0002 ****/ik-hot-dict HTTP/1.1 
common request header
[
    {
        "name":"deploy_0.dic",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_0.dic"
        },
        "sourceType":"OSS",
        "type":"MAIN"
    },
    {
        "name":"deploy_2.dic",
        "ossObject":{
            "bucketName":"search-cloud-test-cn-****",
            "key":"user_dict/dict_2.dic"
        },
        "sourceType":"OSS",
        "type":"STOP"
    },
    {
        "name":"SYSTEM_MAIN.dic",
        "sourceType":"ORIGIN",
         "type":"MAIN"
    },
    {
        "name":"SYSTEM_STOPWORD.dic",
        "sourceType":"ORIGIN",
       "type":"STOP"
    }
]
```

Sample success responses

`JSON`Syntax

```
{
  "Result": [
    {
      "name": "deploy_0.dic",
      "ossObject": {
        "bucketName": "search-cloud-test-cn-****",
        "key": "user_dict/dict_0.dic"
      },
      "sourceType": "OSS",
      "type": "MAIN"
    },
    {
      "name": "deploy_2.dic",
      "ossObject": {
        "bucketName": "search-cloud-test-cn-****",
        "key": "user_dict/dict_2.dic"
      },
      "sourceType": "OSS",
      "type": "STOP"
    },
    {
      "name": "SYSTEM_MAIN.dic",
      "sourceType": "ORIGIN",
      "type": "MAIN"
    },
    {
      "name": "SYSTEM_STOPWORD.dic",
      "sourceType": "ORIGIN",
      "type": "STOP"
    }
  ],
  "RequestId": "E1F6991B-1F77-47EA-9666-593F11E3****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

