# DescribeSnapshotSetting

Call DescribeSnapshotSetting to get the data backup configuration of the cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeSnapshotSetting&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/snapshot-setting HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-0pp1jxvcl000z\*\*\*\*|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|Enable|Boolean|true|Whether to enable automatic backup. |
|QuartzRegex|String|0 0 01 ? \* \* \*|Automatic backup time configuration, using Quartz Cron expression. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-0pp1jxvcl000z****/snapshot-setting HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<RequestId>B2A05BB2-7B66-4D0F-BEDE-5033AFFE****</RequestId>
<Result>
    <Enable>true</Enable>
    <QuartzRegex>0 0 01 ? * * *</QuartzRegex>
</Result>
```

`JSON` format

```
{
    "RequestId": "B2A05BB2-7B66-4D0F-BEDE-5033AFFE****",
    "Result": {
        "Enable": true,
        "QuartzRegex": "0 0 01 ? * * *"
    }
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|404|InstanceNotFound|The specified cluster does not exist. Check the cluster status and try again.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

