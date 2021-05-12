# ListDefaultCollectorConfigurations

Call the ListDefaultCollectorConfigurations to obtain the default configuration file of the collector.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListDefaultCollectorConfigurations&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/beats/default-configurations HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|resType|String|Query|Yes|fileBeat|The collector type. Valid values:

 -   fileBeat
-   metricBeat
-   heartBeat
-   auditBeat |
|resVersion|String|Query|Yes|6.8.5\_with\_community|The collector version. The type of machine deployed by the collector is different, and the optional version is also different. The specific description is as follows:

 -   ECS:6.8.5\_with\_community
-   ACK:6.8.13\_with\_community |
|sourceType|String|Query|No|ECS|Specifies the type of the collector to deploy the machine, and returns all if not. Valid values:

 -   ECS:ECS server
-   ACK: Container Kubernetes cluster |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|8BAE3C32-8E4A-47D6-B4B0-95B5DE643BF5|The ID of the request. |
|Result|Array of Result| |The return results. |
|content|String|- key: log\\n title: Log file content\\n description: \>\\n Contains log file lines.\\n fields:\\n ......|The configuration file content. |
|fileName|String|fields.yml|The name of the configuration file. |

## Examples

Sample requests

```

     GET /openapi/beats/default-configurations?ResVersion=6.8.5 _with_community&resType=fileBeat&sourceType=ECS HTTP/1.1 public request header 
   
```

Sample success responses

`JSON`format

```

     { "Result": [ { "fileName": "fields.yml", "content": "- key: log\n title: Log file content\n description: >\n Contains log file lines.\n fields:\n ......" }, { "fileName": "filebeat.yml", "content": "###################### Filebeat Configuration Example #########################\n\n# This file is an example configuration file ......" } ], "RequestId": "8BAE3C32-8E4A-47D6-B4B0-95B5DE643BF5" } 
   
```

## Error codes

Visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.

