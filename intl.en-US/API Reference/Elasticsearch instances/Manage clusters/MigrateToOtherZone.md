# MigrateToOtherZone

Call the MigrateToOtherZone to migrate the nodes in the specified zone to the destination zone.

If the specifications in your zone are insufficient, you can upgrade your instance to nodes in another zone. Before calling this interface, you must ensure that:

-   The error message returned because the current account is in a zone that has sufficient resources.

    After migrating nodes with current specifications to another zone, you need to manually[upgrade cluster](~~96650~~) because the cluster will not be upgraded during the migration process. Therefore, select a zone with sufficient resources to avoid cluster upgrade failure. We recommend that you choose new zones that are in lower alphabetical order. For example, for cn-hangzhou-e and cn-hangzhou-h zones, choose cn-hangzhou-h first.

-   The cluster is in the healthy state.

    Can be passed`GET _cat/health? V`command to view the health status of the cluster.


## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=MigrateToOtherZone&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request syntax

```
POST /openapi/instances/[InstanceId]/actions/migrate-zones HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|dryRun|Boolean|Yes|false|Verify whether the zone node can be migrated. true indicates that the data is only verified and the migration task is not executed. false indicates that the migration task is executed after the verification is successful. |
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |

## RequestBody

The following parameters must be specified in RequestBody to specify the zone information for migration.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|fromZoneId

|String

|Yes

|cn-hangzhou-i

|The zone where the instance is located. |
|toZoneId

|String

|Yes

|cn-hangzhou-b

|The destination zone to which the instance is to be migrated. |
|toVswitchId

|String

|Yes

|vsw-bp1f7r0ma00pf9h2l\*\*\*\*

|The ID of the VSwitch. |

Example:

```

{
    "fromZoneId": "cn-hangzhou-e",
    "toZoneId": "cn-hangzhou-f",
    "toVswitchId": "vsw-bp16t5hpc689dgkgc****"
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Return results:

-   true: migration succeeded
-   false: The migration fails |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/migrate-zones? dryRun=false HTTP/1.1
Common request parameters
{
    "fromZoneId": "cn-hangzhou-e",
    "toZoneId": "cn-hangzhou-f",
    "toVswitchId": "vsw-bp16t5hpc689dgkgc****"
}
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>24A77388-9444-49A3-A1CF-F48385E5****</RequestId>
```

`JSON` format

```
{
    "Result": true,
    "RequestId": "24A77388-9444-49A3-A1CF-F48385E5****"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

