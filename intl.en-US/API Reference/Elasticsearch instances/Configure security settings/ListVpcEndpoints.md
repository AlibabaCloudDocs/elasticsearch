# ListVpcEndpoints

call the ListVpcEndpoints to view the status of the endpoint in the service vpc.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListVpcEndpoints&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/vpc-endpoints HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-2r429tctl000d\*\*\*\*|The ID of the instance. |
|size|Integer|Query|No|10|The number of entries to return on each page. Default value: 20. |
|page|Integer|Query|No|1|The number of the returned page.

Pages start from page 1. Default value: 1. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC47D9|The ID of the request. |
|Result|Array of Result| |The details of the endpoint information. |
|connectionStatus|String|Disconnected|The endpoint connection status. Valid values:

-   Pending: Modifying.
-   Connecting: The connection is in progress.
-   Connected: connected.
-   Disconnecting: The connection is being disconnected.
-   Disconnected: Not connected.
-   Deleting: The resource group is being deleted.
-   ServiceDeleted: The service corresponding to the endpoint has been deleted. |
|createTime|String|2021-07-22T01:19:24Z|The time when the endpoint was created. |
|endpointBusinessStatus|String|Normal|The business status of the endpoint. Valid values:

-   Normal: working as expected.
-   FinancialLocked: locked due to overdue payments. |
|endpointDomain|String|ep-bp18s6wy9420wdi4\*\*\*\*.epsrv-bp1bz3efowa4kc0\*\*\*\*.cn-hangzhou.privatelink.aliyuncs.com|The endpoint domain name, which is used to configure the connection. |
|endpointId|String|ep-bp1tah7zbrwmkjef\*\*\*\*|The ID of the endpoint. |
|endpointName|String|test|The name of the endpoint. |
|endpointStatus|String|Active|The status of the endpoint. Valid values:

-   Creating: The resource group is being created.
-   Active: available.
-   Pending: Modifying.
-   Deleting: The resource group is being deleted. |
|serviceId|String|epsrv-bp1w0p3jdirbfmt6\*\*\*\*|The ID of the endpoint service that is associated with the endpoint. |
|serviceName|String|com.aliyuncs.privatelink.cn-hangzhou.epsrv-bp1w0p3jdirbfmt6\*\*\*\*|The name of the endpoint service that is associated with the endpoint. |

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-2r429tctl000d****/vpc-endpoints HTTP/1.1 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "endpointId": "ep-bp18s6wy9420wdi4****", "serviceId": "epsrv-bp1bz3efowa4kc03****", "zoneId": "cn-hangzhou-i", "securityGroupId": "sg-bp1emvef6hl69pqs****", "createTime": "2021-07-22T01:19:24Z", "endpointStatus": "Active", "endpointBusinessStatus": "Normal", "endpointDomain": "ep-bp18s6wy9420wdi4****.epsrv-bp1bz3efowa4kc0****.cn-hangzhou.privatelink.aliyuncs.com", "connectionStatus": "Disconnected", "serviceName": "com.aliyuncs.privatelink.cn-hangzhou.epsrv-bp1bz3efowa4kc03****" }, { "endpointId": "ep-bp1tah7zbrwmkjef****", "serviceId": "epsrv-bp1w0p3jdirbfmt6****", "zoneId": "cn-hangzhou-i", "securityGroupId": "sg-bp1emvef6hl69pqs****", "createTime": "2021-07-22T01:12:28Z", "endpointStatus": "Active", "endpointBusinessStatus": "Normal", "endpointDomain": "ep-bp1tah7zbrwmkjef****.epsrv-bp1w0p3jdirbfmt6****.cn-hangzhou.privatelink.aliyuncs.com", "connectionStatus": "Connected", "serviceName": "com.aliyuncs.privatelink.cn-hangzhou.epsrv-bp1w0p3jdirbfmt6****" } ], "RequestId": "AD080217-B2C1-4F36-904F-BE4E4A0B4536" } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

