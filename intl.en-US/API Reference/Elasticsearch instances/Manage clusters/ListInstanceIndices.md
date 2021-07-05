# ListInstanceIndices

Queries the indexes stored on an Elasticsearch cluster. Only independent indexes are displayed.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListInstanceIndices&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters. For more information, see the Common request parameters topic.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/indices HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Location|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w24n9u900am\*\*\*\*|The IDs of the added ECS instances. |
|lang|String|Query|No|en|The configuration language, which supports multiple languages. |
|name|String|Query|No|log-0001|The name of the index. |
|isManaged|Boolean|Query|No|true|Whether to display only the managed index, the value meaning is as follows:

-   true: only indexes in the hosting are displayed.
-   false \(default\): displays all indexes. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Managed-Count|Integer|15|The total number of indexes in cloud hosting. |
|X-Managed-StorageSize|Long|18093942932|The total size of indexes in cloud hosting. Unit: bytes. |
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Array of Result| |Index list details. |
|createTime|String|2021-01-11T05:49:41.114Z|The time when the index list was queried. |
|health|String|green|The running status of the index, which supports the following three states:

-   green: Health.
-   yellow: alarms.
-   red: abnormal. |
|isManaged|String|true|This parameter is deprecated. |
|managedStatus|String|following|The index managed state supports the following three states:

-   following: in hosting.
-   closing: unmanaged.
-   closed: unmanaged. |
|name|String|test1|The name of the index. |
|size|Long|4929858933232|The total storage space occupied by the current index. Unit: bytes. |

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-n6w24n9u900am****/indices?name=log1&isManaged=true HTTP/1.1 
   
```

Sample success responses

`JSON` format

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC****",
    "Result": [
        {
            "name": "test1",
            "health": "green",
            "size": 4929858933232,
            "createTime": "2021-01-11T05:49:41.114Z",
            "managedStatus": "following"
        },
        {
            "name": "test2",
            "health": "yellow",
            "size": 49298589,
            "createTime": "2021-01-11T05:49:41.114Z",
            "managedStatus": "closing"
        }
    ],
    "Headers": {
        "X-Managed-Count": 15,
        "X-Managed-StorageSize": 18093942932
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

