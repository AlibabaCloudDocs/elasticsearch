# Common parameters

This topic describes common parameters, including common request parameters and common response parameters. Common parameters are required by all API operations.

## Common request parameters

Common request parameters must be included in all Elasticsearch API requests.

|Parameter|Example|Required|Description|
|:--------|-------|--------|:----------|
|Authorization|acs <yourAccessKeyId\>:<yourSignature\>|Yes|The authentication information that is used to verify the validity of the request. Specify the information in the `AccessKeyId:Signature` format.|
|Content-Length|0|Yes|The length of the HTTP request body. The length is defined in RFC 2616.|
|Content-Type|application/json|Yes|The type of the HTTP request body. The type is defined in RFC 2616.|
|Content-MD5|0e30656xxxxxxxxx0bc6f70bbdfe|Yes|The Base64-encoded 128-bit MD5 hash value of the HTTP request body. We recommend that you set this parameter for all requests to prevent the requests from being tampered with.|
|Date|Wed, 16 Dec 2015 11:18:47 GMT|Yes|The time when the request was created. The time must be in GMT. If the deviation between the time when the request was sent and the time when the request was received exceeds 15 minutes, the request is considered invalid.|
|Host|cs.aliyuncs.com|Yes|The requested service endpoint. Example: elasticsearch.<RegionId\>.aliyuncs.com.|
|Accept|application/json|Yes|The type of the response that is required by the client. Valid values: application/json and application/xml.|
|x-acs-version|1.0|Yes|The version number of the API. Set the value to 2017-06-13.|
|x-acs-region-id|cn-beijing|Yes|The region ID.|
|x-acs-signature-nonce|f63659d4-10ac-483b-99da-ea8fde61eae3|Yes|A unique, random number used to prevent replay attacks. You must use different numbers for different requests.|
|x-acs-signature-method|HMAC-SHA1|Yes|The encryption method of the signature string. Set the value to HMAC-SHA1.|

Sample request:

```
GET /clusters HTTP/1.1
Host: cs.aliyuncs.com
Accept: application/json
User-Agent: cs-sdk-python/0.0.1 (Darwin/15.2.0/x86_64;2.7.10)
x-acs-signature-nonce: f63659d4-10ac-483b-99da-ea8fde******
Authorization: acs <yourAccessKeyId>:<yourSignature>
x-acs-signature-version: 1.0
Date: Wed, 16 Dec 2015 11:18:47 GMT
x-acs-signature-method: HMAC-SHA1
Content-Type: application/json;charset=utf-8
X-Acs-Region-Id: cn-beijing
Content-Length: 0
```

## Common response parameters

Every response returns a unique RequestID regardless of whether the call is successful. API responses use a unified format. API responses use the HTTP response format where a `2xx` status code indicates a successful call and a `4xx` or `5xx` status code indicates a failed call.

Responses can be returned in either the JSON or XML format. You can specify the response format in the request. The default response format is JSON.

XML format

```
<?xml version="1.0" encoding="UTF-8"?>
<!—Result Root Node-->
<Interface Name+Response>
 | <!—Return Request Tag-->
 | <RequestId>4C467B38-3910-447D-87BC-AC049166F216</RequestId>
 | <!—Return Result Data-->
</Interface Name+Response>
```

JSON format

```
{
    "RequestId": "4C467B38-3910-447D-87BC-AC049166F216"
    /* Return Result Data */
}
```

