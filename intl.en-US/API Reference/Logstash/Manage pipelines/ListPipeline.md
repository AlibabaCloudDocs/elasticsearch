# ListPipeline

Queries the pipelines of a Logstash cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListPipeline&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/logstashes/[InstanceId]/pipelines HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|ls-cn-oew1qbgl\*\*\*\*|The Logstash instance ID. |
|pipelineId|String|Query|No|pipeline\_test|The ID of the pipeline. |
|page|Integer|Query|No|1|The number of pages of the returned result. Default value: 1, minimum value: 1, maximum value: 200. |
|size|Integer|Query|No|15|The number of pipes per page. Minimum value: 1, maximum value: 200. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|2|The total number of entries. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|gmtCreatedTime|String|2020-08-05T03:10:38.188Z|pipeline creation time. |
|gmtUpdateTime|String|2020-08-05T08:43:31.757Z|The pipeline update time. |
|pipelineId|String|pipeline\_test|The ID of the pipeline. |
|pipelineStatus|String|NOT\_DEPLOYED|Pipeline status, supported: NOT\_DEPLOYED, RUNNING, and DELETED. |

## Examples

Sample requests

```

     GET /openapi/logstashes/ls-cn-oew1qbgl****/pipelines?pipelineId=test HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "pipelineId": "datahub_test", "pipelineStatus": "RUNNING", "gmtCreatedTime": "2020-09-09T02:21:28.844Z", "gmtUpdateTime": "2020-09-09T06:09:43.796Z" }, { "pipelineId": "test", "pipelineStatus": "NOT_DEPLOYED", "gmtCreatedTime": "2020-09-16T06:35:30.139Z", "gmtUpdateTime": "2020-09-16T07:06:24.759Z" }, { "pipelineId": "test_1", "pipelineStatus": "NOT_DEPLOYED", "gmtCreatedTime": "2020-09-16T06:33:05.290Z", "gmtUpdateTime": "2020-09-16T06:33:05.290Z" } ], "RequestId": "AA6771E2-4007-4F1F-ADCB-16DAABB9****", "Headers": { "X-Total-Count": 3 } } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

