# UntagResources

Deletes a user resource tag relationship.

When you call this operation, take note of the following items:

-   You can only delete user tags.

**Note:** User labels are manually added to instances by users. A system Tag is a tag that Alibaba Cloud services add to instances. System labels are divided into visible labels and invisible labels.

-   If you delete a resource tag relationship that is not associated with any resources, you must delete the tags.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UntagResources&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
DELETE /openapi/tags HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|ResourceIds|String|Required|\["es-cn-09k1rocex0006\*\*\*\*","es-cn-oew1rgiev0009\*\*\*\*"\]|The resource list that you want to delete. |
|ResourceType|String|Required|INSTANCE|The type of the resource. Fixed to **INSTANCE** . |
|TagKeys|String|No|\["tagKey1","tagKey2"\]|The list of tags that you want to delete. The list can contain up to 20 subitems. |
|All|Boolean|No|false|Specifies whether to delete all parts. Default value: **false** . This parameter is valid only when **TagKeys** is not specified. |

-   **All=true** you call this operation with when TagKeys is empty and all resource tag relationships under the Resource are deleted. For resources without tags, the interface is not processed and success is returned.
-   When **TagKeys** is null and **All=false**, the API is not processed and success is returned.
-   **TagKeys** is not empty. **All** is ignored when this operation is called, whether it is true or false.
-   **After the TagKeys**specified**are**are, you can call this operation to delete the specified tags from resources. If the specified tag does not exist on a resource, the specified tag is not processed.
-   If the requested resource does not exist, the system returns **InvalidResourceId.NotFound** when you call this operation.

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6D\*\*\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   true: deleted
-   false: Failed |

## Examples

Sample requests

```
DELETE /openapi/tags? ResourceType=INSTANCE&ResourceIds=%5B%22es-cn-09k1rocex0006****%22%5D HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result":true,
    "RequestId": "3D8795D9-8FF5-46B2-86E6-E3B40*******"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

