# TriggerNetwork

Call TriggerNetwork to enable or disable public or private network access for the Elasticsearch or Kibana cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=TriggerNetwork&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
POST /openapi/instances/[InstanceId]/actions/network-trigger HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|nodeType|String|Body|No|KIBANA|The type of the instance. Valid values:

-   KIBANA:Kibana cluster
-   WORKER: a Elasticsearch cluster |
|networkType|String|Body|No|PUBLIC|The type of the network. Valid values:

-   PUBLIC: PUBLIC network
-   PRIVATE: PRIVATE network |
|actionType|String|Body|No|OPEN|The type of the action. Valid values:

-   CLOSE: disable
-   OPEN: enable |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Boolean|true|Indicates whether SQL audit was disabled for the DRDS database. |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c ****/actions/network-trigger HTTP/1.1 
common request header
```

Sample success responses

`XML` format

```
<Result>true</Result>
<RequestId>5A5D8E74-565C-43DC-B031-29289F****</RequestId> 
```

`JSON` Syntax

```
{
    "Result": true,
    "RequestId": "5A5D8E74-565C-43DC-B031-29289F****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

