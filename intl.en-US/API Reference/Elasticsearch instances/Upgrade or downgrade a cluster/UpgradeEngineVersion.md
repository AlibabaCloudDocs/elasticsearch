# UpgradeEngineVersion

Upgrades the version or kernel of an Elasticsearch cluster. You can upgrade the version of an Elasticsearch cluster only from V6.3 to V6.7.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpgradeEngineVersion&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     POST /openapi/instances/[InstanceId]/actions/upgrade-version HTTP/1.1 
   
```

## Request parameters

|Attribute|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|dryRun|Boolean|Query|No|false|Whether to perform pre-upgrade verification. true indicates verification, and false indicates no verification. |
|version|String|Body|No|6.7|Upgraded version, optional values: 6.7, ali1.2.0. the value is 6.7, the type must be a engineVersion; value ali1.2.0, the type must be a aliVersion. |
|type|String|Body|No|engineVersion|Upgrade type, optional values: engineVersion \(version upgrade, default\), aliVersion \(patch upgrade\). |

## Response parameters

|Attribute|Type|Sample response|Description|
|---------|----|---------------|-----------|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DC\*\*\*\*\*|The ID of the request. |
|Result|Struct| |The returned results. |
|status|String|success|Check whether it passes. Success means pass, failed means not. |
|validateResult|Struct| |Check the information. |
|errorCode|String|ClusterStatusNotHealth|The error code returned. |
|errorMsg|String|The cluster status is not health|The error message returned. |
|errorType|String|clusterStatus|The type of the error that occurred. Valid values:

-   clusterStatus: cluster health status
-   clusterConfigYml: Cluster YML File
-   clusterConfigPlugins: Cluster Configuration File
-   clusterResource: cluster resources
-   clusterSnapshot: cluster snapshot |
|validateType|String|checkClusterHealth|The monitoring type. Valid values:

-   checkClusterHealth: cluster health status
-   checkConfigCompatible: Configure Compatibility Status
-   checkClusterResource: resource space status
-   checkClusterSnapshot: whether a snapshot exists |

## Examples

Sample requests

```

     POST /openapi/instances/es-cn-n6w1o1x0w001c **** /actions/upgrade-version HTTP/1.1 public request header {"version": "ali1.2.0", "type": "aliVersion" } 
   
```

Sample success responses

`JSON` format

```

     { "Result":[ { "validateType":"checkClusterHealth", "status":"failed", "validateResult":[ { "errorType":"clusterStatus", "errorCode":"ClusterStatusNotHealth", "errorMsg":"ClusterStatusNotHealth" } ] }, { "validateType":"checkConfigCompatible", "status":"failed", "validateResult":[ { "errorType":"clusterConfigYml", "errorCode":"ClusterYamlNotCompatible", "errorMsg":"ClusterYamlNotCompatible" }, { "errorType":"clusterConfigPlugins", "errorCode":"ClusterPluginsNotSupport", "errorMsg":"ClusterPluginsNotSupport" } ] }, { "validateType":"checkClusterResource", "status":"failed", "validateResult":[ { "errorType":"clusterResource", "errorCode":"ClusterResourceNotEnough", "errorMsg":"ClusterResourceNotEnough" } ] }, { "validateType":"checkClusterSnapshot", "status":"failed", "validateResult":[ { "errorType":"clusterSnapshot", "errorCode":"ClusterSnapshotNotAvaild", "errorMsg":"ClusterSnapshotNotAvaild" } ] } ], "RequestId":"F99407AB-2FA9-489E-A259-40CF6DC****" } 
   
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceActivating|Instance is activating.|The instance is currently in effect.|
|404|InstanceNotFound|The instanceId provided does not exist.|The instance cannot be found. Please check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

