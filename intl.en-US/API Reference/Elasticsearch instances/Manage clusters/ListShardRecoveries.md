# ListShardRecoveries

You can call ListShardRecoveries to return a list of data progress about ongoing and completed part recovery. By default, information about ongoing part recovery is returned.

**Note:** Shard recovery is the process of synchronizing from primary to secondary shards. After the restoration is complete, the secondary parts are available for searching.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListShardRecoveries&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/cat-recovery HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-7mz293m9a003j\*\*\*\*|The ID of the instance. |
|activeOnly|Boolean|Query|No|true|Displays the multipart data recovery tracking. The values are as follows:

-   true: displays the data recovery trace of the sharding data in progress.
-   false: displays the tracking status of all shard data recovery. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC47D9|The ID of the request. |
|Result|Array of Result| |The return result of the video moderation task. |
|bytesPercent|String|80%|The progress of data restoration. |
|bytesTotal|Long|12086|The total amount of data restored. |
|filesPercent|String|80.0%|File execution progress. |
|filesTotal|Long|79|The total number of files. |
|index|String|my-index-000001|The name of the index. |
|sourceHost|String|192.168.XX.XX|The IP address of the source node. |
|sourceNode|String|2Kni3dJ|The source node. |
|stage|String|done|The status of the data recovery phase. The values are as follows:

-   done: The execution is completed.
-   finalize: cleanup.
-   index: reads index metadata and copies bytes from the source to the destination.
-   init: The recovery has not yet started.
-   start: starts the recovery.
-   translog: redo transaction logs. |
|targetHost|String|192.168.XX.XX|The IP address of the destination node. |
|targetNode|String|YVVKLmW|The ID of the object to which you want to attach the control policy. |
|translogOps|Long|12086|The number of Translog operations to be restored. |
|translogOpsPercent|String|80%|Restore the progress of the Translog operation. |

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-7mz293m9a003j****/cat-recovery HTTP/1.1 
   
```

Sample success responses

`JSON` format

```

     { "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC47D9", "Result": [ { "index": "my-index-000001", "sourceHost": "192.168.XX.XX", "sourceNode": "2Kni3dJ", "targetHost": "192.168.XX.XX", "targetNode": "YVVKLmW", "stage": "index", "filesTotal": 79, "filesPercent": "80.0%", "bytesTotal": 12086, "bytesPercent": "80%", "translogOps": 12086, "translogOpsPercent": "80%" }, { "index": "my-index-000002", "sourceHost": "192.168.XX.XX", "sourceNode": "2Kni3dJ", "targetHost": "192.168.XX.XX", "targetNode": "YVVKLmW", "stage": "index", "filesTotal": 56, "filesPercent": "100%", "bytesTotal": 11086, "bytesPercent": "100%", "translogOps": 11086, "translogOpsPercent": "100%" } ] } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

