# ValidateSlrPermission

Call the ValidateSlrPermission to verify that the service linked role has been created.

**Note:** Before you use the collector tool to collect logs from different data sources, you must be authorized to create service linked roles. You can call this operation to verify that it has been created.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ValidateSlrPermission&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request structure

```
GET /openapi/user/servicerolepermission HTTP/1.1  
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|rolename|String|Query|Yes|AliyunServiceRoleForElasticsearchCollector|The name of the service linked role. Valid values:

-   AliyunServiceRoleForElasticsearchCollector: create and manage Beats collectors |
|ClientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BC4ED7DD-8C84-49B5-8A95-456F82E44D13|The ID of the request. |
|Result|Boolean|true|Indicates whether the service linked role was created. Valid values:

-   true: Created
-   false: not created |

## Examples

Sample requests

```
GET /openapi/user/servicerolepermission?rolename=AliyunServiceRoleForElasticsearchCollector HTTP/1.1 
common request headers
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>2C77A9B5-6B2A-42D7-9DBB-0166A0D40483</RequestId>
```

`JSON` format

```
{
  "Result": true,
  "RequestId": "2C77A9B5-6B2A-42D7-9DBB-0166A0D40483"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

