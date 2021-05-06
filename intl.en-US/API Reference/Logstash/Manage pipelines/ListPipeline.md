# ListPipeline

Call the ListPipeline to obtain the list of pipelines of the Logstash instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=ListPipeline&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/pipelines HTTPS|HTTP 
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the Logstash instance. |
|pipelineId|String|Yes|pipeline\_test|The ID of the pipeline. |
|page|Integer|No|1|The number of pages in the returned result. |
|size|Integer|No|15|The number of returned record per page. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|2|The total number of entries. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|gmtCreatedTime|String|2020-08-05T03:10:38.188Z|The time when the pipeline was created. |
|gmtUpdateTime|String|2020-08-05T08:43:31.757Z|The time when the pipeline was updated. |
|pipelineId|String|pipeline\_test|The ID of the pipeline. |
|pipelineStatus|String|NOT\_DEPLOYED|The status of the pipeline. Valid values: NOT\_DEPLOYED, RUNNING, and DELETED. |

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/pipelines?pipelineId=test HTTP/1.1 
common request header
```

Sample success responses

`XML` format

```
<Result>
    <pipelineId>datahub_test</pipelineId>
    <pipelineStatus>RUNNING</pipelineStatus>
    <gmtCreatedTime>2020-09-09T02:21:28.844Z</gmtCreatedTime>
    <gmtUpdateTime>2020-09-09T06:09:43.796Z</gmtUpdateTime>
</Result>
<Result>
    <pipelineId>test</pipelineId>
    <pipelineStatus>NOT_DEPLOYED</pipelineStatus>
    <gmtCreatedTime>2020-09-16T06:35:30.139Z</gmtCreatedTime>
    <gmtUpdateTime>2020-09-16T07:06:24.759Z</gmtUpdateTime>
</Result>
<Result>
    <pipelineId>test_1</pipelineId>
    <pipelineStatus>NOT_DEPLOYED</pipelineStatus>
    <gmtCreatedTime>2020-09-16T06:33:05.290Z</gmtCreatedTime>
    <gmtUpdateTime>2020-09-16T06:33:05.290Z</gmtUpdateTime>
</Result>
<RequestId>AA6771E2-4007-4F1F-ADCB-16DAABB9****</RequestId>
<Headers>
    <X-Total-Count>3</X-Total-Count>
</Headers>
```

`JSON` Syntax

```
{
	"Result": [
		{
			"pipelineId": "datahub_test",
			"pipelineStatus": "RUNNING",
			"gmtCreatedTime": "2020-09-09T02:21:28.844Z",
			"gmtUpdateTime": "2020-09-09T06:09:43.796Z"
		},
		{
			"pipelineId": "test",
			"pipelineStatus": "NOT_DEPLOYED",
			"gmtCreatedTime": "2020-09-16T06:35:30.139Z",
			"gmtUpdateTime": "2020-09-16T07:06:24.759Z"
		},
		{
			"pipelineId": "test_1",
			"pipelineStatus": "NOT_DEPLOYED",
			"gmtCreatedTime": "2020-09-16T06:33:05.290Z",
			"gmtUpdateTime": "2020-09-16T06:33:05.290Z"
		}
	],
	"RequestId": "AA6771E2-4007-4F1F-ADCB-16DAABB9****",
	"Headers": {
		"X-Total-Count": 3
	}
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

