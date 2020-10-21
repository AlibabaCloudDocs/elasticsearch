# ListAllNode

Call ListAllNode to query the information about all nodes in the cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListAllNode&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/nodes HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|cpuPercent|String|4.1%|The CPU usage. |
|diskUsedPercent|String|2.0%|The disk usage. |
|health|String|GREEN|The health status of the node. Valid values: GREEN, YELLOW, RED, and GRAY. |
|heapPercent|String|47.9%|The JVM memory. |
|host|String|10.6.15.32|The IP address of the node. |
|loadOneM|String|0.09|Load per minute. |
|nodeType|String|WORKER|The type of the nodes. Valid values:

-   MASTER: dedicated MASTER node
-   WORKER: Hot node
-   WORKER\_WARM: warm node
-   COORDINATING: client node
-   KIBANA: Kibana node |
|port|Integer|9200|The node access port. |
|zoneId|String|cn-hangzhou-g|The zone where the node is located. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/nodes HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <nodeType>WORKER_WARM</nodeType>
    <host>172.16.xx.xx</host>
    <port>9200</port>
    <zoneId>cn-hangzhou-i</zoneId>
    <heapPercent>70.8%</heapPercent>
    <cpuPercent>4.1%</cpuPercent>
    <loadOneM>0.09</loadOneM>
    <diskUsedPercent>2.0%</diskUsedPercent>
    <health>GREEN</health>
</Result>
<Result>
    <nodeType>WORKER_WARM</nodeType>
    <host>172.16.xx.xx</host>
    <port>9200</port>
    <zoneId>cn-hangzhou-i</zoneId>
    <heapPercent>45.0%</heapPercent>
    <cpuPercent>3.3%</cpuPercent>
    <loadOneM>0.00</loadOneM>
    <diskUsedPercent>3.0%</diskUsedPercent>
    <health>GREEN</health>
</Result>
<Result>
    <nodeType>WORKER</nodeType>
    <host>172.16.xx.xx</host>
    <port>9200</port>
    <zoneId>cn-hangzhou-i</zoneId>
    <heapPercent>53.4%</heapPercent>
    <cpuPercent>3.7%</cpuPercent>
    <loadOneM>0.02</loadOneM>
    <diskUsedPercent>1.0%</diskUsedPercent>
    <health>GREEN</health>
</Result>
<RequestId>76387A8A-5ED9-43B4-AC90-76146209****</RequestId>
```

`JSON` format

```
{
    "Result": [
        {
            "nodeType": "WORKER_WARM",
            "host": "172.16.xx.xx",
            "port": 9200,
            "zoneId": "cn-hangzhou-i",
            "heapPercent": "70.8%",
            "cpuPercent": "4.1%",
            "loadOneM": "0.09",
            "diskUsedPercent": "2.0%",
            "health": "GREEN"
        },
        {
            "nodeType": "WORKER_WARM",
            "host": "172.16.xx.xx",
            "port": 9200,
            "zoneId": "cn-hangzhou-i",
            "heapPercent": "45.0%",
            "cpuPercent": "3.3%",
            "loadOneM": "0.00",
            "diskUsedPercent": "3.0%",
            "health": "GREEN"
        },
        {
            "nodeType": "WORKER",
            "host": "172.16.xx.xx",
            "port": 9200,
            "zoneId": "cn-hangzhou-i",
            "heapPercent": "53.4%",
            "cpuPercent": "3.7%",
            "loadOneM": "0.02",
            "diskUsedPercent": "1.0%",
            "health": "GREEN"
        }
    ],
    "RequestId": "76387A8A-5ED9-43B4-AC90-76146209****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

