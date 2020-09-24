# Make API requests

To send an Elasticsearch API request, you must send an HTTP request to the Elasticsearch endpoint. You must add the request parameters that correspond to the API operation being called. After you call the API operation, the system returns a response. The request and response are encoded in UTF-8.

## Endpoints

The endpoint of the Elasticsearch API is elasticsearch.<RegionId\>.aliyuncs.com.

## Protocols

You can send requests over HTTP or HTTPS. For security purposes, we recommend that you send requests over HTTPS.

## Request syntax

The request syntax is as follows:

```
HTTPMethod /resource_URI_parameters
RequestHeader
RequestBody
```

-   HTTPMethod: the method of the HTTP request, such as PUT, POST, PATCH, GET, and DELETE.
-   resource\_URI\_parameters: the identifier of the resource you want to access, such as `/cluster`.
-   RequestHeader: the request header. It contains information such as the API version, hostname, and authentication information. For more information, see [Common parameters]().
-   RequestBody: the request parameters. The parameters include common request parameters and operation-specific parameters. The common request parameters contain information such as the API version and authentication information.

Sample request:

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/recover-zones HTTP/1.1
Common request parameters
["cn-hangzhou-i","cn-hangzhou-h"]
```

## Encoding

The request and response are encoded in UTF-8.

