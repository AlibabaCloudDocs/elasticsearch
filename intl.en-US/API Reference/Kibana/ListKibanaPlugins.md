# ListKibanaPlugins

Queries the plug-ins of Kibana.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListKibanaPlugins&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances/[InstanceId]/kibana-plugins HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-oew1q8bev0002\*\*\*\*|The ID of the instance. |
|page|String|Query|No|1|The page number of the returned page. Default value: 1. |
|size|Integer|Query|No|10|The number of entries to return on each page. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The request header. |
|X-Total-Count|Integer|3|The number of returned data bars. |
|RequestId|String|11234B4A-34CE-473B-8E61-AD95702E\*\*\*\*|The ID of the request. |
|Result|Array of PluginItem| |current requests that return information about your plug-in. |
|description|String|Customize DSL statements to query data.|The description of the plug-in. |
|name|String|bsearch\_querybuilder|The name of the plug-in. |
|source|String|SYSTEM|The source of the plug-in. |
|specificationUrl|String|https://xxxx|The plug-in introduction address, which supports null. |
|state|String|INSTALLED|The plug-in installation status. |

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-oew1q8bev0002****/kibana-plugins?Page=1&size=10 HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "name": "bsearch_label", "state": "UNINSTALLED", "source": "SYSTEM", "description": "Mark data in a visual way to complete the query tasks quickly and easily.", "specificationUrl": "https://xxx.html" }, { "name": "bsearch_querybuilder", "state": "UNINSTALLED", "source": "SYSTEM", "description": "Customize DSL statements to query data.", "specificationUrl": "https://xxx.html" }, { "name": "network_vis", "state": "UNINSTALLED", "source": "SYSTEM", "description": "This is a plugin developed for Kibana that displays a network node that link two fields that have been previously selected." } ], "RequestId": "11234B4A-34CE-473B-8E61-AD95702E****", "Headers": { "X-Total-Count": 3 } } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Please check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

