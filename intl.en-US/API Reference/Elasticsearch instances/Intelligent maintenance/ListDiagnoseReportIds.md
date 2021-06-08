# ListDiagnoseReportIds

Queries the IDs of the historical intelligent O&M reports of an Elasticsearch cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListDiagnoseReportIds&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/diagnosis/instances/[InstanceId]/report-ids HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|endTime|Long|Query|Yes|1595174399999|query end timestamps. |
|InstanceId|String|Path|Yes|es-cn-n6w1qu7ei000p\*\*\*\*|The ID of the instance. |
|startTime|Long|Query|Yes|1595088000000|the query start timestamps. |
|lang|String|Query|No|spanish|The language of the obtained report. |
|page|Integer|Query|No|1|The number of the returned page. Default value: 1, minimum value: 1, maximum value: 200. |
|size|Integer|Query|No|15|The number of report ID per page. Default value: 10, minimum value: 1, maximum value: 500. |
|trigger|String|Query|No|SYSTEM|The trigger method of health diagnosis, support: SYSTEM \(system automatic trigger\), INNER \(internal trigger\) and USER \(user manual trigger\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|1|The total number of entries returned. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|List|\["scheduled\_\_2020-09-13T00:40:00"\]|The return results. |

## Examples

Sample requests

```

     GET /openapi/diagnosis/instances/es-cn-09k1rocex0006****/report-ids?startTime=1600099200000&endTime=1600185600000 HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ "scheduled__2020-09-15T00:40:00" ], "RequestId": "B5F822C5-03E9-4899-8D80-2B35515A****", "Headers": { "X-Total-Count": 1 } } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

