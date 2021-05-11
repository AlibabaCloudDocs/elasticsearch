# EstimatedRestartTime

Call the EstimatedRestartTime to obtain the estimated time for restarting the instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=EstimatedRestartTime&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/estimated-time/restart-time HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|force|Boolean|No|false|Indicates whether the restart is forced. Default value: false. |

## RequestBody

You can also configure the following parameters in RequestBody to specify the restart parameters.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|restartType

|String

|Yes

|instance

|The restart type. Valid values: instance \(default value: instance\) and nodeIp \(node restart\). |
|nodes

|List<String\\\>

|No

|\["127.0.0.1"\]

|Select the IP address list of the target node when the node restarts. |
|blueGreenDep

|Boolean

|No

|false

|Specifies whether to perform a blue-green change when restarting a node. Default value: false. |
|batch

|Integer

|No

|25.0

|The concurrency of forced instance restart. Default value: 1/The number of nodes in an instance. |
|batchUnit

|String

|No

|percent

|batch unit. Default value: percent. |

-   When restartType is set to instance, the blueGreenDep parameter is ignored.
    -   If the value of the force parameter is true, the value of the batch parameter must be greater than 0 but less than or equal to 100. Otherwise, an RestartBatchValueError error occurs.
    -   If the value of the force parameter is false, the default value of the batch parameter is 0. If you set this parameter to another value, an error NormalRestartNotSupportBatch.

-   The batch parameter is ignored when the restartType parameter is set to nodeIp.
    -   If nodeIp is empty, the system prompts a Parameter error.
    -   If blueGreenDep is set to true, the system restarts for the blue-green change. If the parameter is set to false, the system restarts normally.

Example:

```

{
    "restartType":"nodeIp",
    "nodes": ["172.16.xx.xx"],
    "blueGreenDep":true
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|unit|String|second|The unit. |
|value|Long|50|Estimated restart time. |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/estimated-time/restart-time? force=true HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": {
        "unit": "second",
        "value": 4200
    },
    "RequestId": "7ACE8751-DD1B-40DB-A253-9080CA58****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

