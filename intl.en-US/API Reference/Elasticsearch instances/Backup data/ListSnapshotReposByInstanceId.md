# ListSnapshotReposByInstanceId

Call the ListSnapshotReposByInstanceId to get the cross-cluster OSS repositories of the current instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListSnapshotReposByInstanceId&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/snapshot-repos HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-0pp1jxvcl000z\*\*\*\*|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|The ID of the request. |
|Result|Array of repo| |The return results. |
|instanceId|String|es-cn-6ja1ro4jt000c\*\*\*\*|Reference instance ID. |
|repoPath|String|es-cn-6ja1ro4jt000c\*\*\*\*|The address of the repository. |
|snapWarehouse|String|aliyun\_snapshot\_from\_es-cn-6ja1ro4jt000c\*\*\*\*|Reference warehouse name. |
|status|String|available|Reference warehouse status. available indicates that it is valid. unavailable indicates that it is invalid. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-0pp1jxvcl000z****/snapshot-repos HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": [
        {
            "instanceId": "es-cn-6ja1ro4jt000c****",
            "snapWarehouse": "aliyun_snapshot_from_es-cn-6ja1ro4jt000c****",
            "repoPath": "es-cn-6ja1ro4jt000c****",
            "status": "available"
        }
    ],
    "RequestId": "123BB496-6EEF-41E6-92BB-3F782664****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

