# ListAlternativeSnapshotRepos

Call ListAlternativeSnapshotRepos to get the OSS reference warehouses that can be added to the current instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListAlternativeSnapshotRepos&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/alternative-snapshot-repos HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-0pp1jxvcl000z\*\*\*\*|The ID of the instance. |
|alreadySetItems|Boolean|No|true|Indicates whether to return the OSS reference repository added. The return value. Valid values: true and false. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of repo| |The return results. |
|instanceId|String|es-cn-6ja1ro4jt000c\*\*\*\*|The ID of the instance. |
|repoPath|String|RepoPath|The address of the repository. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-0pp1jxvcl000z****/alternative-snapshot-repos HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <instanceId>es-cn-6ja1ro4jt000c****</instanceId>
    <repoPath>es-cn-6ja1ro4jt000c****</repoPath>
</Result>
<Result>
    <instanceId>es-cn-oew1rgiev0009****</instanceId>
    <repoPath>es-cn-oew1rgiev0009****</repoPath>
</Result>
<RequestId>335D2540-BB16-447F-8AD4-39B7A0AE****</RequestId>
```

`JSON` format

```
{
    "Result": [
        {
            "instanceId": "es-cn-6ja1ro4jt000c****",
            "repoPath": "es-cn-6ja1ro4jt000c****"
        },
        {
            "instanceId": "es-cn-oew1rgiev0009****",
            "repoPath": "es-cn-oew1rgiev0009****"
        }
    ],
    "RequestId": "335D2540-BB16-447F-8AD4-39B7A0AE****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

