# AddSnapshotRepo

Call the AddSnapshotRepo to create a reference repository when configuring a cross-cluster OSS repository.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=AddSnapshotRepo&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only.

## Request syntax

```
POST /openapi/instances/[InstanceId]/snapshot-repos HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance for which you want to access OSS repositories across clusters. |

## RequestBody

The following parameters must be set in RequestBody to specify the cross-cluster backup information.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|repoPath

|String

|Yes

|es-cn-4591jumei000u\*\*\*\*

|The ID of the instance for which you want to restore data. After this parameter is specified, Elasticsearch creates a snapshot of the instance from which you can restore data.

The instance and the source instance must meet the following requirements:

Instances within the same region belong to the same account, and the version of the source instance is earlier than or equal to the version of the destination instance. For more information, see [configure cross-cluster OSS repositories](~~131441~~). |

Example:

```

{
    "repoPath" :"es-cn-4591jumei000u****"
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   true: Reference warehouse created successfully
-   false: Reference warehouse created failed |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/snapshot-repos HTTP/1.1
{
    "repoPath" :"es-cn-4591jumei000u****"
}
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>D21379E3-A54E-4C86-A64C-3717365F****</RequestId>
```

`JSON` format

```
{
    "Result": true,
    "RequestId": "D21379E3-A54E-4C86-A64C-3717365F****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

