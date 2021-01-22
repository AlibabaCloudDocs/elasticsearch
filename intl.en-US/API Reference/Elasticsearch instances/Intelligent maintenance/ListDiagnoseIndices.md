# ListDiagnoseIndices

Call the ListDiagnoseIndices to obtain the health diagnosis diagnostic index in the specified instance Intelligent O&M module.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=ListDiagnoseIndices&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/diagnosis/instances/[InstanceId]/indices HTTP/1.1
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|lang|String|Query|No|en|Language configuration. Multiple languages are supported. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F05ED12E-140A-4ACB-B059-3A508A69F2E1|The ID of the request. |
|Result|List|\["my\_index\_aliws", "aliyun-index-test","filebeat-6.7.0-2020.11.15", "filebeat-6.7.0-2020.12.27"\]|The list of diagnostic indexes. |

## Examples

Sample requests

```
GET /openapi/diagnosis/instances/es-cn-n6w1o1x0w001c ****/indices HTTP/1.1 
common request header 
```

Sample success responses

`XML` format

```
<Result>my_index_aliws</Result>
<Result>aliyun-index-test</Result>
<Result>filebeat-6.7.0-2020.11.15</Result>
<Result>filebeat-6.7.0-2020.12.27</Result>
<RequestId>F05ED12E-140A-4ACB-B059-3A508A69F2E1</RequestId>
```

`JSON` Syntax

```
{
    "Result": [
        "my_index_aliws",
        "aliyun-index-test",
        "filebeat-6.7.0-2020.11.15",
        "filebeat-6.7.0-2020.12.27"
    ],
    "RequestId": "F05ED12E-140A-4ACB-B059-3A508A69F2E1"
}
```

## Error codes

For more information about error codes, see [error center](https://error-center.alibabacloud.com/status/product/elasticsearch).

