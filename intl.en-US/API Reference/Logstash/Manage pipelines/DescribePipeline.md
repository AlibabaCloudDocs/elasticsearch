# DescribePipeline

Call the DescribePipeline to obtain the pipeline information of a Logstash instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=DescribePipeline&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/pipelines/[PipelineId] HTTPS|HTTP 
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the Logstash instance. |
|PipelineId|String|Yes|pipeline\_test|The ID of the pipeline. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|829F38F6-E2D6-4109-90A6-888160BD1\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|batchDelay|Integer|50|Pipeline batch delay. |
|batchSize|Integer|125|Pipeline batch size. |
|config|String|input \{ \} filter \{ \} output \{ \}|The specific configuration of the pipeline. |
|description|String|this is a test|The description of the pipeline. |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|The time when the pipeline was created. |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|The time when the pipeline was updated. |
|pipelineId|String|pipeline\_test|The ID of the pipeline. |
|pipelineStatus|String|RUNNING|The status of the pipeline. Supported: NOT\_DEPLOYED, RUNNING, and DELETED. |
|queueCheckPointWrites|Integer|1024|The number of queue checkpoint writes. |
|queueMaxBytes|Integer|1024|The maximum number of bytes in the queue. |
|queueType|String|memory|The type of the queue. Supported: MEMORY and PERSISTED. |
|workers|Integer|2|The number of pipeline workers. |

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/pipelines/pipeline_test HTTP/1.1 
common request header
```

Sample success responses

`JSON` Syntax

```
{
    "Result": {
        "pipelineId": "test",
        "config": "input {\n\n}\nfilter {\n\n}\noutput {\n\n}",
        "workers": 2,
        "batchSize": 100,
        "batchDelay": 60,
        "queueType": "MEMORY",
        "pipelineStatus": "NOT_DEPLOYED",
        "queueMaxBytes": 1024,
        "queueCheckPointWrites": 1024,
        "gmtCreatedTime": "2020-09-16T06:35:30.139Z",
        "gmtUpdateTime": "2020-09-16T07:06:24.759Z"
    },
    "RequestId": "49AEA9F3-B946-4102-8BD7-92E71C86****"
}
```

```

     { "Result": { "pipelineId": "test", "config": "input {\n\n}\nfilter {\n\n}\noutput {\n\n}", "workers": 2, "batchSize": 100, "batchDelay": 60, "queueType": "MEMORY", "pipelineStatus": "NOT_DEPLOYED", "queueMaxBytes": 1024, "queueCheckPointWrites": 1024, "gmtCreatedTime": "2020-09-16T06:35:30.139Z", "gmtUpdateTime": "2020-09-16T07:06:24.759Z" }, "RequestId": "49AEA9F3-B946-4102-8BD7-92E71C86****" } 
   
```

Sample response description

```
Pipeline parameters that are not configured use the system default values, and are not displayed in the returned result.  
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

