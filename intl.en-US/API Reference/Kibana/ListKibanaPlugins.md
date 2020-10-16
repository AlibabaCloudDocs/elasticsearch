# ListKibanaPlugins

Call the ListKibanaPlugins to obtain the Kibana plug-in list.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListKibanaPlugins&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/kibana-plugins HTTPS|HTTP
```

## Request Parameter

|Prameter|Type|Required|Example|Description|
|--------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-oew1q8bev0002\*\*\*\*|The ID of the instance. |
|page|String|No|1|The number of the page to return. Minimum value: 1. Default value: 1. |
|size|Integer|No|10|The number of entries to return on each page. Maximum Value: 50. Default value: 20. |

## Response parameters

|Parameter|Type|Sample response|Description|
|---------|----|---------------|-----------|
|Headers|Struct| |The request header. |
|X-Total-Count|Integer|3|The number of returned data entries. |
|RequestId|String|11234B4A-34CE-473B-8E61-AD95702E\*\*\*\*|The ID of the request. |
|Result|Array of PluginItem| |The returned plug-in information of the current request. |
|description|String|Customize DSL statements to query data.|The description of the plug-in. |
|name|String|bsearch\_querybuilder|The name of the plug-in. |
|source|String|SYSTEM|The source of the plug-in. |
|specificationUrl|String|https://xxxx|The introduction address of the plug-in. null is supported. |
|state|String|INSTALLED|The agent installation status. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-oew1q8bev0002****/kibana-plugins? page=1&size=10 HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <name>bsearch_label</name>
    <state>UNINSTALLED</state>
    <source>SYSTEM</source>
    <description>Mark data in a visual way to complete the query tasks quickly and easily.</description>
    <specificationUrl>https://xxx.html</specificationUrl>
</Result>
<Result>
    <name>bsearch_querybuilder</name>
    <state>UNINSTALLED</state>
    <source>SYSTEM</source>
    <description>Customize DSL statements to query data.</description>
    <specificationUrl>https://xxx.html</specificationUrl>
</Result>
<Result>
    <name>network_vis</name>
    <state>UNINSTALLED</state>
    <source>SYSTEM</source>
    <description>This is a plugin developed for Kibana that displays a network node that link two fields that have been previously selected.</description>
</Result>
<RequestId>11234B4A-34CE-473B-8E61-AD95702E****</RequestId>
<Headers>
    <X-Total-Count>3</X-Total-Count>
</Headers>
```

`JSON` format

```
{
    "Result": [
        {
            "name": "bsearch_label",
            "state": "UNINSTALLED",
            "source": "SYSTEM",
            "description": "Mark data in a visual way to complete the query tasks quickly and easily.",
            "specificationUrl": "https://xxx.html"
        },
        {
            "name": "bsearch_querybuilder",
            "state": "UNINSTALLED",
            "source": "SYSTEM",
            "description": "Customize DSL statements to query data.",
            "specificationUrl": "https://xxx.html"
        },
        {
            "name": "network_vis",
            "state": "UNINSTALLED",
            "source": "SYSTEM",
            "description": "This is a plugin developed for Kibana that displays a network node that link two fields that have been previously selected."
        }
    ],
    "RequestId": "11234B4A-34CE-473B-8E61-AD95702E****",
    "Headers": {
        "X-Total-Count": 3
    }
}
```

## Error codes

|HttpCode|Error codes|Error message|Description|
|--------|-----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

