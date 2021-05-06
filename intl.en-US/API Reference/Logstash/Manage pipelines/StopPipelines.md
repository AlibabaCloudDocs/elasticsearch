StopPipelines {#title}
======================

Call the StopPipelines to stop running the Logstash pipeline. 

Debugging 
------------------------------

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=StopPipelines&type=ROA&version=2017-06-13)

Request header 
-----------------------------------

This operation uses only common request headers. For more information, see Common parameters.

Request syntax 
-----------------------------------

    POST /openapi/logstashes/[InstanceId]/pipelines/action/stop HTTPS|HTTP



Request parameters 
---------------------------------------



|  Parameter  |  Type  | Required |                Example                 |                                                                                                          Description                                                                                                          |
|-------------|--------|----------|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InstanceId  | String | Yes      | ls-cn-oew1qbgl\*\*\*\*                 | The ID of the Logstash instance.                                                                                                                                                                                              |
| ClientToken | String | No       | 5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\* | A unique token generated by the client to guarantee the idempotency of the request. The value of this parameter is generated by the client and is unique among different requests. The maximum length is 64 ASCII characters. |



RequestBody 
--------------------------------

Enter the pipeline ID list in RequestBody to specify the pipeline to be deployed. Example: ` ["PipelineId1","PipelineId2","..."] `.

Response parameters 
----------------------------------------



| Parameter |  Type   |                 Example                  |                                                                                  Description                                                                                   |
|-----------|---------|------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RequestId | String  | 5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\* | The ID of the request.                                                                                                                                                         |
| Result    | Boolean | true                                     | Returned results: * true: stopped successfully   * false: stopped failed    |



Examples 
-----------------------------

Sample requests

    POST /openapi/logstashes/ls-cn-oew1qbgl****/pipelines/action/stop HTTP/1.1
    common request header
    ["PipelineId1","PipelineId2","..."]



Sample success responses



```

```



` JSON ` Syntax

    {
            "Result": true,
            "RequestId": "57092D4B-C92E-4CF9-B73C-B4B376C9****"
    }



Error code 
-------------------------------

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).