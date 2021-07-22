# ListInstanceIndices

Queries the indexes stored on an Elasticsearch cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListInstanceIndices&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/indices HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-tl329rbpc0001\*\*\*\*|The ID of the instance. |
|all|Boolean|Query|No|false|Specifies whether to obtain all indexes. Valid values:

-   true: returns a list of indexes including system indexes.
-   false \(default\): returns a list of indexes other than the system index. |
|name|String|Query|No|.ds-ds--2021.07.21-000002|The name of the index. |
|isManaged|Boolean|Query|No|true|Specifies whether to display only the indexes in the host. The values are as follows:

-   true: only the indexes in the hosting are displayed.
-   false \(default\): displays all indexes. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Managed-Count|Integer|1|The total number of indexes in Cloud Hosting. |
|X-Managed-StorageSize|Long|416|The total size of the index in Cloud Hosting. Unit: byte. |
|RequestId|String|11608354-FBA1-4177-A13A-FAC7E5D0\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The details of the index list. |
|createTime|String|2021-01-11T05:49:41.114Z|The time when the index list was queried. |
|health|String|green|The running status of the index. The following three statuses are supported:

-   green: healthy.
-   yellow: alerts.
-   red: an exception. |
|isManaged|String|true|This parameter is deprecated. |
|managedStatus|String|following|The managed status of the index. The following three statuses are supported:

-   following: Hosting.
-   closing: The host is being canceled.
-   closed: unmanaged. |
|name|String|.ds-ds--2021.07.21-000002|The name of the index. |
|size|Long|416|The total storage space occupied by the current index. Unit: byte. |

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-tl329rbpc0001****/indices HTTP/1.1 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "name": ".ds-ds--2021.07.21-000002", "health": "green", "size": 416, "createTime": "2021-07-21T14:40:37.098Z", "managedStatus": "following" } ], "RequestId": "11608354-FBA1-4177-A13A-FAC7E5D0F73F", "Headers": { "X-Managed-Count": 1, "X-Managed-StorageSize": 416 } } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

