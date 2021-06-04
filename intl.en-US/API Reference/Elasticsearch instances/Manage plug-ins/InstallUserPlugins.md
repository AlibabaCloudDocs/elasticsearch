# InstallUserPlugins

Call the InstallUserPlugins to install user-defined plug-ins that have been uploaded to the Elasticsearch console.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=InstallUserPlugins&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     POST /openapi/instances/[InstanceId]/plugins/user/actions/install HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-i7m27ausp001l\*\*\*\*|The ID of the instance. |

## RequestBody

The following parameters must be filled in the RequestBody to specify a list of custom plug-ins.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|RequestBody

|Array

| | | |
|└ name

|String

|Yes

|pluginName1.zip

|User-defined plug-ins that have been uploaded to the Elasticsearch console. |

└ indicates a child parameter.

For example, an error message is returned if you use the following code:

```

     [ {"name": "pluginName1.zip"}, {"name": "pluginName2.zip"} ] 
   
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6F\*\*\*\*\*|The ID of the request. |
|Result|List|\["pluginName1.zip", "pluginName2.zip"\]|The list of plug-ins that are requested to be installed. If the installation fails, the system will return the corresponding error code. Please refer to the error code to locate the problem. |

## Examples

Sample requests

```

     POST /openapi/instances/[es-cn-i7m27ausp001l ****]/plugins/user/actions/install HTTP/1.1 public request header ''' [ {"name": "pluginName1.zip"}, {"name": "pluginName2.zip"} ] 
   
```

Sample success responses

`JSON` format

```

     { "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6F*****", "Result": "[\"pluginName1.zip\", \"pluginName2.zip\"]" } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceActivating|Instance is activating.|The instance is currently in effect.|
|400|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Please check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

