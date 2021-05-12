# ListNodes

Call ListNodes to view the status of the ECS machine where the collector is installed.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListNodes&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/collectors/[ResId]/nodes HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ResId|String|Path|Yes|ct-cn-l871nd0u73c45\*\*\*\*|The collector ID. |
|page|Integer|Query|No|1|The number of pages of the returned result. |
|size|Integer|Query|No|10|The number of results per page. |
|ecsInstanceIds|String|Query|No|i-bp1ei8ysh7orb6eq\*\*\*\*|The ID of ECS instance N. |
|ecsInstanceName|String|Query|No|test|The ECS instance name. |
|tags|String|Query|No|\[\{"tagKey":"abc","tagValue":"xyz"\}\]|The tag information of the ECS instance. The tag key \(tagKey\) and tag value \(tagValue\) must be included. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|10|The number of entries returned. |
|RequestId|String|E1FD7642-7C40-4FF2-9C0F-21F1A1746F70|The ID of the request. |
|Result|Array of Result| |The return results. |
|agentStatus|String|heartOk|The status of each collector on ECS. Then, you can perform the following operations:

-   heartOk: normal heartbeat
-   heartLost: abnormal heartbeat
-   uninstalled: not installed
-   failed: installation failed |
|cloudAssistantStatus|String|true|Whether the Cloud Assistant has been opened. Then, you can perform the following operations:

-   true: Open
-   false: not enabled |
|ecsInstanceId|String|i-bp13y63575oypr\*\*\*\*|The ID of the instance. |
|ecsInstanceName|String|ECS\_beat|The ECS instance name. |
|ipAddress|Array of ipAddress| |The IP information list of the ECS instance. |
|host|String|192.168.xx.xx|The IP address. |
|ipType|String|public|The type of the IP address. Then, you can perform the following operations:

-   public: public IP address
-   private: private IP address |
|osType|String|linux|The operating system type of the ECS instance. Then, you can perform the following operations:

-   windows:Windows Server
-   linux:Linux |
|status|String|running|The status of the ECS instance. Then, you can perform the following operations:

-   running: The cluster is running.
-   starting
-   stopping: Stopping
-   stopped: The nodes are stopped. |
|tags|Array of tags| |The tag information of the ECS instance. |
|tagKey|String|abc|The key of the tag. |
|tagValue|String|xyz|The tag value. |

## Examples

Sample requests

```

     GET /openapi/collectors/ct-cn-l871nd0u73c45 ****/nodes HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "ecsInstanceId": "i-bp13y63575oypr9d****", "ecsInstanceName": "zl-test02-keepit", "status": "running", "ipAddress": [ { "host": "47.111.xx.xx", "ipType": "public" }, { "host": "10.8.xx.xx", "ipType": "private" } ], "tags": [ { "tagKey": "a", "tagValue": "b" } ], "agentStatus": "failed", "osType": "linux", "cloudAssistantStatus": "true" } ], "RequestId": "E1FD7642-7C40-4FF2-9C0F-21F1A1746F70", "Headers": { "X-Total-Count": 1 } } 
   
```

## Error codes

Visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.

