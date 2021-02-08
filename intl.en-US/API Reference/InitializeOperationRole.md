# InitializeOperationRole

Call InitializeOperationRole to create a service linked role.

**Note:** When you use a collector to collect logs from different data sources or perform cluster auto scaling tasks, you must first create service linked roles. For more information, see [Overview of the Elasticsearch service linked role](~~172624~~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=InitializeOperationRole&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
POST /openapi/user/slr HTTP/1.1
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ClientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

Set the following parameters in RequestBody to specify the name of the service-linked role to be created.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|rolename

|String

|Yes

|AliyunServiceRoleForElasticsearchCollector

|The name of the service linked role. Valid values: AliyunServiceRoleForElasticsearchOps \(the role that performs the elastic scaling task of the cluster\) and AliyunServiceRoleForElasticsearchCollector \(creates and manages Beats collectors\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|29101430-4797-4D1D-96C3-9FCBCCA8F845|The ID of the request. |
|Result|Boolean|true|Indicates whether SQL audit was disabled for the DRDS database. Supported:

-   true: creation succeed
-   false: creation failed |

## Examples

Sample requests

```
POST /openapi/user/slr HTTP/1.1 
common request header
{
  "rolename": "AliyunServiceRoleForElasticsearchCollector"
} 
```

Sample success responses

`XML`format

```
<Result>true</Result>
<RequestId>29101430-4797-4D1D-96C3-9FCBCCA8F845</RequestId>
```

`JSON`Syntax

```
{
    "Result": true,
    "RequestId": "29101430-4797-4D1D-96C3-9FCBCCA8F845"
}
```

## Error codes

