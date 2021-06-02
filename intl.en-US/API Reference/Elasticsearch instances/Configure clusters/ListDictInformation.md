# ListDictInformation

The call ListDictInformation obtains and verifies the details of the user OSS dictionary file when adding the dictionary file stored by the user OSS.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListDictInformation&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/dict/_info HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|bucketName|String|Query|Yes|search-cloud-test-cn-\*\*\*\*|The name of the OSS bucket where the dictionary file resides. |
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|key|String|Query|Yes|oss/dic\_0.dic|The path where the dictionary file is stored in OSS Bucket. |
|analyzerType|String|Query|No|ALIWS|The OSS dictionary type to be added by the user. Four types of IK\_HOT, IK, SYNONYMS, and ALIWS are supported. Default value: IK |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|7C4334EA-D22B-48BD-AE28-08EE68\*\*\*\*\*\*|The ID of the request. |
|Result|Struct| |The returned results. |
|fileSize|Long|2202301|The dictionary file size, unit: Byte. |
|ossObject|Struct| |OSS Open Storage file details. |
|bucketName|String|es-osstest\*|The name of the bucket where the OSS storage file is located. |
|etag|String|2ABAB5E70BBF631145647F6BE533\*\*\*\*|The MD5 check code Etag \(uppercase\) of the OSS storage file. |
|key|String|oss/dict\_0\*.dic|The path where the dictionary file is stored in OSS Bucket. |
|type|String|STOP|Thesaurus type, which supports the following two types:

-   MAIN: main participle thesaurus
-   STOP: disables thesaurus |

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-n6w1o1x0w001c****/dict/_info?bucketName=your-oss-bucket-name&key=test/dict.dic&analyzerType=ALIWS HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "RequestId": "7C4334EA-D22B-48BD-AE28-08EE68******", "Result": { "fileSize": 2202301, "type": "STOP", "ossObject": { "bucketName": "es-osstest*", "etag": "2ABAB5E70BBF631145647F6BE533****", "key": "oss/dict_0*.dic" } } } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.

