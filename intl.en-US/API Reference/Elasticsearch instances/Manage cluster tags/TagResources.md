# TagResources

Call TagResources to create a relationship between tag resources.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=TagResources&type=ROA&version=2017-06-13)

## Request header

https://elasticsearch.cn-hangzhou.aliyuncs.com/openapi/tags

## Request syntax

```
POST /openapi/tags HTTPS|HTTP
```

## Request parameters

The request parameters is empty, but the RequestBody parameter is required.

Specify the required parameters in RequestBody to specify the target resource and tags.

|Field

|Type

|Required

|Example

|Description |
|-------|------|----------|---------|-------------|
|ResourceIds

|Array

|Yes

|\["es-cn-aaa","es-cn-bbb"\]

|The ID of the resource. |
|Tags

|Array

|Yes

|\[\{"key":"env","value":"IT"\}\]

|The list of newly created tags. A maximum of 20 child tags are allowed. |
|ResourceType

|String

|Yes

|INSTANCE

|The type of the resource. Set this parameter to INSTANCE. |

**Note:**

-   Except that you must pass this parameter, if the key specified in the Tags parameter exists, then this value must exist and can be an empty string. Otherwise, a InvalidParameter.TagKey error is returned.
-   If the input key-value pairs are the same, the Duplicate.TagKey error is reported.
-   If a Tag key already exists before custom Tags are defined, the value of the previous Tag key is overwritten.

Example:

```

{
    "ResourceIds":["es-cn-oew1q8bev0002****","es-cn-09k1ptccp0009****"],
    "Tags": [
        {
            "key": "env",
            "value": "IT"
        }
    ],
    "ResourceType": "INSTANCE"
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|3D8795D9-8FF5-46B2-86E6-E3B407\*\*\*\*\*\*\*|The ID of the request. |

The response parameters also include the **Result** parameter. The value is of the Boolean type. If the resource relationship is created, true is returned. If the resource relationship is failed, false is returned.

## Examples

Sample requests

```
POST /openapi/tags HTTP/1.1
Common request parameters
{
    "ResourceIds":["es-cn-oew1q8bev0002****","es-cn-09k1ptccp0009****"],
    "Tags": [
        {
            "key": "env",
            "value": "IT"
        }
    ],
    "ResourceType": "INSTANCE"
}
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>27627E6B-E26A-406F-B6E1-0247882C****</RequestId>
```

`JSON` format

```
{
    "Result": true,
    "RequestId": "27627E6B-E26A-406F-B6E1-0247882C****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

