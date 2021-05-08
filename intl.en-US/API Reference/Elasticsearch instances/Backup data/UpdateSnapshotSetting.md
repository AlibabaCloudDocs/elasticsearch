# UpdateSnapshotSetting

Call UpdateSnapshotSetting to update the data backup configuration of the specified instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateSnapshotSetting&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST|PUT /openapi/instances/[InstanceId]/snapshot-setting HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-oew1rgiev0009\*\*\*\*|The ID of the instance. |

## RequestBody

The following parameters must be set in RequestBody to specify the modified backup data.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|quartzRegex

|String

|No

|0 0 01 ? \* \* \*

|The start time of automatic backup. When enable is true, it is required. |
|enable

|Boolean

|Yes

|true

|Specifies whether to enable scheduled backups. |

Example:

```

{
    "quartzRegex":"0 0 01 ? * * *",
    "enable":true
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|enable|Boolean|true|Specifies whether to enable automatic backup. |
|quartzRegex|String|0 0 01 ? \* \* \*|The start time of automatic backup. |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-oew1rgiev0009****/snapshot-setting HTTP/1.1
Common request parameters
{
    "quartzRegex":"0 0 01 ? * * *",
    "enable":true
}
```

Sample success responses

`JSON` format

```
{
    "Result": {
        "quartzRegex": "0 0 01 ? * * *",
        "enable": true
    },
    "RequestId": "77C0C894-77E3-4711-88E1-495216FA****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

