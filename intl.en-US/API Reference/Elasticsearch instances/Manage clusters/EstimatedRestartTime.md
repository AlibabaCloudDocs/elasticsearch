# EstimatedRestartTime

Call EstimatedRestartTime to obtain the estimated duration of the instance restart.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=EstimatedRestartTime&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request syntax

```
POST /openapi/instances/[InstanceId]/estimated-time/restart-time HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|Force|Boolean|No|false|Whether to force restart. |

## RequestBody

You can also enter the following parameters in RequestBody to specify restart parameters.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|restartType

|String

|No

|instance

|The restart type. Valid values: instance and node IP. Default value: instance. |
|nodes

|List<String\\\>

|No

|\["127.0.0.1"\]

|The list of IP addresses for the target node when the node restarts. |
|blueGreenDep

|Boolean

|No

|false

|Specifies whether to perform blue-green upgrade when the node restarts. Default value: false. |
|batch

|Double

|No

|25.0

|The concurrency of forced instance restart. |
|batchUnit

|String

|No

|percent

|The unit of batch processing. Default value: percent. |

-   When restartType is set to instance, the blueGreenDep parameter is ignored.
    -   If the value of the force parameter is true, the value of the batch parameter must be greater than 0 but less than or equal to 100. Otherwise, a RestartBatchValueError error is returned.
    -   If force is set to false, the default value of batch is 0. If another value is set, a NormalRestartNotSupportBatch error is returned.

-   When restartType is set to nodeIp, the batch parameter is ignored.
    -   If the nodeIp field is empty, a Parameter error occurs.
    -   Blue-green restart is performed after a the blueGreenDep parameter is set to true, and normal restart is set to false.

Sample request:

```

{
    "restartType":"instance",
    "batch":"20.0",
    "batchUnit":"percent"
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results of the OCR task. |
|unit|String|second|The unit. |
|value|Long|50|Estimated restart time. |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/estimated-time/restart-time? Force=true HTTP/1.1
Common request parameters
{
    "restartType":"instance",
    "batch":"20.0",
    "batchUnit":"percent"
}
```

Sample success responses

`XML` format

```
<Result>
    <unit>second</unit>
    <value>4200</value>
</Result>
<RequestId>7ACE8751-DD1B-40DB-A253-9080CA58****</RequestId>
```

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

