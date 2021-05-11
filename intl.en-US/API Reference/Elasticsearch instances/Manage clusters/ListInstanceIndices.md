# ListInstanceIndices

Call ListInstanceIndices to get the index list of the cluster, this can filter system indexes.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListInstanceIndices&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request syntax

```
GET /openapi/diagnosis/instances/[InstanceId]/indices HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|lang|String|No|en|The configuration language. Multiple languages are supported. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|List|\["index1","index2","index3"\]|The returned result, that is the index list. |

## Examples

Sample requests

```
GET /openapi/diagnosis/instances/es-cn-n6w1o1x0w001c****/indices HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": [
        "filebeat-6.7.0-2020.09.12",
        "filebeat-6.7.0-2020.07.30",
        "test",
        "filebeat-6.7.0-2020.09.11",
        "filebeat-6.7.0-2020.08.16",
        "my_index_aliws"
    ],
    "RequestId": "5BF8C741-26F8-4F67-B399-E49E1A05****"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

