# DescribeKibanaSettings

Call DescribeKibanaSettings to obtain the Kibana configuration.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeKibanaSettings&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/kibana-settings HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6D\*\*\*\*\*|The ID of the request. |
|Result|Map|\{"map.includeElasticMapsService": "false", "server.ssl.cert": "/home/admin/packages/kibana/config/cert/client.crt", "server.ssl.enabled": "true", "server.ssl.key": "/home/admin/packages/kibana/config/cert/client.key", "xpack.reporting.capture.browser.chromium.disableSandbox": "true"\}|Return results:

-   true: The Kibana configuration obtained successfully
-   false: The Kibana configuration obtained failed |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/kibana-settings HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": {
        "map.includeElasticMapsService": "false",
        "server.ssl.cert": "/home/admin/packages/kibana/config/cert/client.crt",
        "server.ssl.enabled": "true",
        "server.ssl.key": "/home/admin/packages/kibana/config/cert/client.key",
        "xpack.reporting.capture.browser.chromium.disableSandbox": "true"
    },
    "RequestId": "131834B6-AE89-45D4-878B-D2F46A8C****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

