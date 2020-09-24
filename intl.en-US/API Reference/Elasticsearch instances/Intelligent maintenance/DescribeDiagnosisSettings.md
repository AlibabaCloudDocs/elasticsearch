# DescribeDiagnosisSettings

Call DescribeDiagnosisSettings to obtain the scenario settings of intelligent maintenance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeDiagnosisSettings&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request structure

```
GET /openapi/diagnosis/instances/[InstanceId]/settings HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-68n1n8b7f000a\*\*\*\*|The ID of an instance. |
|lang|String|No|en|The language of the returned result. Default value: en. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5E82B8A8-EED7-4557-A6E9-D1AD3E58\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|scene|String|Business Search|Scenarios of intelligent maintenance. |
|updateTime|Long|1588994035385|The timestamp of the last update for Intelligent Maintenance scenarios. |

## Examples

Sample requests

```
GET /openapi/diagnosis/instances/es-cn-45914gy290009****/settings HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <scene>Business Search</scene>
    <updateTime>1561969800000</updateTime>
</Result>
<RequestId>592414F7-652B-4360-91AB-85C53B32****</RequestId>
```

`JSON` format

```
{
    "Result": {
        "scene": "Business Search",
        "updateTime": 1561969800000
    },
    "RequestId": "592414F7-652B-4360-91AB-85C53B32****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

