# ListPipelineIds

When calling the ListPipelineIds to set up Kibana pipeline management, test the connectivity between Logstash and Kibana and obtain the list of pipeline IDs created on the target Kibana.

**Note:** Pipeline management is divided into configuration file management and Kibana pipeline management. Kibana pipeline management is not open in some regional consoles.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListPipelineIds&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     POST /openapi/instances/[InstanceId]/pipeline-ids HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|ls-cn-7g1umu96oit2e\*\*\*\*|The Logstash instance ID. |

## RequestBody

The following parameters must be filled in the RequestBody to verify the Kibana information of the management pipeline.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|userName

|String

|Yes

|elastic

|The username that is used to log on to the Kibana console. Default value: elastic. |
|password

|String

|Yes

|xxxxxx

|The password used to log on to the Kibana console. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of Result| |Returns a list of results. |
|available|Boolean|true|Whether the pipeline is available or not, the value is as follows:

-   true: Available.
-   flase: unavailable. |
|code|String|OK|The pipeline is unavailable for error codes. |
|message|String|OK|Pipeline unavailable error message. |
|pipelineId|String|testKibanaManagement|The ID of the pipeline created on Kibana. |

## Examples

Sample requests

```

     POST /openapi/instances/ls-cn-7g1umu96oit2e ****/pipeline-ids HTTP/1.1 public request header {"userName":"elastic", "password":"xxxxxx"} 
   
```

Sample success responses

`JSON`format

```

     { "Result":[ { "pipelineId":"testKibanaManagement", "available":true, "code":"OK", "message":"OK" } ], "RequestId":"E50BC6C3-23B5-4CA0-983C-066098FB8E34" } 
   
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

