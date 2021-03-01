# UpdateExtendfiles

Call the UpdateExtendfiles to update the extended file configurations of a Logstash instance.

Note the following when calling this interface:

Currently, this operation only allows you to delete Logstash extension files that have been uploaded in the console. If you want to add or modify an identifier, perform the operations in the console.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateExtendfiles&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request structure

```
PUT /openapi/logstashes/[InstanceId]/extendfiles HTTP/1.1 
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the cluster. |
|ClientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

To specify the updated extension file configuration, enter the following parameters in RequestBody:

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|name

|String

|Yes

|mysql-connector-java-6.0.2.jar

|The name of the extended file. The file suffix must be .jar. Chinese characters are not supported in the file name, and the length cannot exceed 100 characters. |
|sourceType

|String

|Yes

|ORIGIN

|The extended file Source. Currently, only ORIGIN is supported. That is, the corresponding extension file is retained. The extension file that does not have this parameter configured is deleted. The function of adding and modifying extended files is in development. You can implement all management and control operations in control. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|fileSize|Long|1853083|The size of the extended file. Unit: Byte. |
|name|String|mysql-connector-java-6.0.2.jar|The name of the extended file. |
|sourceType|String|ORIGIN|The source of the extended file. Only the ORIGIN \(Original Extended file retained\) is supported. |

## Examples

Sample requests

```
PUT /openapi/logstashes/ls-cn-oew1qbgl****/extendfiles HTTP/1.1 common request header
[
    {
        "sourceType":"ORIGIN",
        "name":"mysql-connector-java-5.1.48.jar"
    }
] 
```

Sample success responses

`XML` format

```
<Result>
    <name>mysql-connector-java-5.1.35.jar</name>
    <fileSize>968668</fileSize>
    <sourceType>ORIGIN</sourceType>
</Result>
<RequestId>27F32ECF-0527-43BF-A116-D6260D1240BE</RequestId> 
```

`JSON` format

```
{
  "Result": [
    {
      "name": "mysql-connector-java-5.1.35.jar",
      "fileSize": 968668,
      "sourceType": "ORIGIN"
    }
  ],
  "RequestId": "27F32ECF-0527-43BF-A116-D6260D1240BE"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

