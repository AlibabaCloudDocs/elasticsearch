# EstimatedRestartTime

Obtains the estimated time that is required to restart an Elasticsearch cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=EstimatedRestartTime&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     POST /openapi/instances/[InstanceId]/estimated-time/restart-time HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the cluster. |
|force|Boolean|Query|No|false|Whether it is a forced restart. Default: false. |

## RequestBody

You can also enter the following parameters in the RequestBody to specify the restart parameter information.

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

|Restart type, support: instance \(restart instance\), nodeIp \(node restart\). Default value: Restart the instance. |
|nodes

|List<String\\\>

|No

|\["127.0.0.1"\]

|Select the IP address list of the target node when the node restarts. |
|blueGreenDep

|Boolean

|No

|false

|Whether to make a blue-green change when the node is restarted. Default value: false. |
|batch

|Integer

|No

|25.0

|The degree of concurrency of instance forced restart. Default value: 1 /Total number of nodes of the instance. |
|batchUnit

|String

|No

|percent

|The batch unit. Default value: percent. |

-   The blueGreenDep parameter is ignored when the restartType is instance.
    -   Force is true,batch must be greater than 0 and less than or equal to 100, otherwise the system will prompt the RestartBatchValueError to report an error.
    -   Force is false and batch defaults to 0. When other values are entered, an error NormalRestartNotSupportBatch will be reported.

-   The batch parameter is ignored when restartType is nodeIp.
    -   If nodeIp is empty, the system will prompt a parameter error.
    -   BlueGreenDep is true, and the blue-green change is restarted; false, normal restart.

Sample template:

```

     { "restartType":"nodeIp", "nodes": ["172.16.xx.xx"], "blueGreenDep":true } 
   
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The returned results. |
|unit|String|second|The unit. |
|value|Long|50|Estimated restart time. |

## Examples

Sample requests

```

     POST /openapi/instances/es-cn-n6w1o1x0w001c****/estimated-time/restart-time?force=true HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": { "unit": "second", "value": 4200 }, "RequestId": "7ACE8751-DD1B-40DB-A253-9080CA58****" } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

