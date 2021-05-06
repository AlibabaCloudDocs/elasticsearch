# ListLogstashPlugins

Call ListLogstashPlugins to obtain the details of all or specified plug-ins.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. You can use OpenAPI Explorer to search for API operations, call API operations, and dynamically generate SDK sample code.](https://api.aliyun.com/#product=elasticsearch&api=ListLogstashPlugins&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/logstashes/[InstanceId]/plugins HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|ls-cn-oew1qbgl\*\*\*\*|The ID of the instance. |
|name|String|No|logstash-filter-clone|The name of the plug-in. |
|page|Integer|No|10|The number of pages in the plug-in list. Pages start from page 1. Default value: 1. |
|size|Integer|No|3|The number of entries to return on each page. Maximum value: 100. Default value: 10. |
|source|String|No|USER|The source of the plug-in. Valid values:

-   USER: Custom plug-ins
-   SYSTEM: SYSTEM preset plug-ins |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|description|String|The clone filter is for duplicating events.|The description of the plug-in. |
|name|String|logstash-filter-clone|The name of the plug-in. |
|source|String|SYSTEM|The source of the plug-in. |
|specificationUrl|String|https://xxx.html|Address of the documentation for the plug-in. |
|state|String|INSTALLED|The status of the plug-in. |

The returned data also contains the following parameters.

|Parameter

|Type

|Example

|Description |
|-----------|------|---------|-------------|
|Headers

|Struct

| |The header of the response. |
|└X-Total-Count

|Integer

|131

|The number of returned plug-ins. |

**Note:** └ indicates a child parameter.

## Examples

Sample requests

```
GET /openapi/logstashes/ls-cn-oew1qbgl****/plugins?name=logstash-filter-clone&page=10&size=3 HTTP/1.1
common request header
```

Sample success responses

`JSON` Syntax

```
{
    "Result": [
        {
            "name": "logstash-filter-clone",
            "state": "INSTALLED",
            "source": "SYSTEM",
            "description": "The clone filter is for duplicating events. A clone will be created for each type in the clone list.",
            "specificationUrl": "https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-clone.html"
        },
        {
            "name": "logstash-filter-csv",
            "state": "INSTALLED",
            "source": "SYSTEM",
            "description": "The CSV filter takes an event field containing CSV data, parses it, and stores it as individual fields (can optionally specify the names). This filter can also parse data with any separator, not just commas.",
            "specificationUrl": "https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-csv.html"
        },
        {
            "name": "logstash-filter-date",
            "state": "INSTALLED",
            "source": "SYSTEM",
            "description": "The date filter is used for parsing dates from fields, and then using that date or timestamp as the logstash timestamp for the event.",
            "specificationUrl": "https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-date.html"
        }
    ],
    "RequestId": "40C0570B-AB40-48BA-8CAE-66EA230A****",
    "Headers": {
        "X-Total-Count": 131
    }
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the instance cannot be found. Check the status of the instance.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

