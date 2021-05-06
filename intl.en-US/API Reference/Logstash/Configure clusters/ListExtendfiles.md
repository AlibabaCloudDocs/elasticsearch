# ListExtendfiles

Call the ListExtendfiles to obtain the extended file configurations of a Logstash instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=ListExtendfiles&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/extendfiles HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the Logstash instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|filePath|String|/ssd/1/share/ls-cn-oew1qbgl\*\*\*\*/logstash/current/config/custom/mysql-connector-java-5.1.35.jar|The path of the extended file. |
|fileSize|Long|968668|Expands the file size. |
|name|String|mysql-connector-java-5.1.35.jar|The name of the extended file. |
|sourceType|String|ORIGIN|The source of the synonym dictionary file. |

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-oew1qbgl ****/extendfiles HTTP/1.1
common request header 
```

Sample success responses

`JSON` Syntax

```
{
    "Result": [
        {
            "name": "mysql-connector-java-5.1.35.jar",
            "fileSize": 968668,
            "sourceType": "ORIGIN",
            "filePath": "/ssd/1/share/ls-cn-oew1qbgl****/logstash/current/config/custom/mysql-connector-java-5.1.35.jar"
        }
    ],
    "RequestId": "741099F7-F490-4679-A52E-38601EE7****"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

