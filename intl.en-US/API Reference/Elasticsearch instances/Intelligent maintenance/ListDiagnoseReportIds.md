# ListDiagnoseReportIds

Call ListDiagnoseReportIds to obtain the ID of the historical intelligent maintenance report.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListDiagnoseReportIds&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request structure

```
GET /openapi/diagnosis/instances/[InstanceId]/report-ids HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|endTime|Long|Yes|1595174399999|The end time of the query. |
|InstanceId|String|Yes|es-cn-n6w1qu7ei000p\*\*\*\*|The ID of an instance. |
|startTime|Long|Yes|1595088000000|The start time of the query. |
|lang|String|No|spanish|The language of the report obtained. |
|page|Integer|No|1|The page number of the returned page. |
|size|Integer|No|15|Number of report IDs per page. |
|trigger|String|No|SYSTEM|The trigger method of the health diagnosis. Valid values: SYSTEM \(automatically triggered by the SYSTEM\), INNER \(internally triggered\), and USER \(manually triggered by the USER\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|1|The number of records returned. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|List|\["scheduled\_\_2020-09-13T00:40:00"\]|The return results. |

## Examples

Sample requests

```
GET /openapi/diagnosis/instances/es-cn-09k1rocex0006****/report-ids? startTime=1600099200000&endTime=1600185600000 HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>scheduled__2020-09-15T00:40:00</Result>
<RequestId>B5F822C5-03E9-4899-8D80-2B35515A****</RequestId>
<Headers>
    <X-Total-Count>1</X-Total-Count>
</Headers>
```

`JSON` format

```
{
    "Result": [
        "scheduled__2020-09-15T00:40:00"
    ],
    "RequestId": "B5F822C5-03E9-4899-8D80-2B35515A****",
    "Headers": {
        "X-Total-Count": 1
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

