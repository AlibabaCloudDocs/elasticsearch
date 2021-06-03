# GetRegionConfiguration

Obtains the open configuration information of the current region. The return value of the interface is for reference. The actual value displayed on the console and sales page shall prevail.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=GetRegionConfiguration&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/region HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|zoneId|String|Query|No|cn-hangzhou|The current region ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6F\*\*\*\*\*\*|The ID of the request. |
|Result|Struct| |The region configuration information returned. |
|clientNodeAmountRange|Struct| |The range of the number of coordinated node nodes. |
|maxAmount|Integer|25|The maximum number of coordination node nodes. |
|minAmount|Integer|2|The minimum number of coordination node nodes. |
|clientNodeDiskList|Array of disk| |Coordinate node disk allowable values. |
|diskType|String|cloud\_efficiency|The storage type of the disk. |
|maxSize|Integer|20|The maximum value allowed for a disk. |
|minSize|Integer|20|The minimum value allowed for a disk. |
|scaleLimit|Integer|18|The disk allows setting the maximum value of consecutive values. |
|clientNodeSpec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.4xlarge","elasticsearch.ic5.xlarge","elasticsearch.ic5.2xlarge","elasticsearch.ic5.3xlarge","elasticsearch.ic5.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge"\]|Coordination node specifications. |
|createUrl|String|https://common-buy.aliyun.com/?commodityCode=elasticsearch&orderType=BUY\#/buy|Sales page entry address. |
|dataDiskList|Array of dataDiskList| |Data node disk allowable value. |
|diskType|String|cloud\_ssd|The storage type of the disk. |
|maxSize|Integer|5120|The maximum value allowed for a disk. |
|minSize|Integer|20|The minimum value allowed for a disk. |
|scaleLimit|Integer|2048|The disk allows setting the maximum value of consecutive values. |
|valueLimitSet|List|\[2560,3072,3584,4096,4608,5120\]|The discrete value allowed by the disk. |
|elasticNodeProperties|Struct| |Elastic node configurations. |
|amountRange|Struct| |The value of the number of cold node nodes. |
|maxAmount|Integer|25|The maximum number of nodes. |
|minAmount|Integer|2|The minimum number of nodes. |
|diskList|Array of disk| |The list of disk configurations. |
|diskEncryption|Boolean|true|Whether to allow disk encryption. The meaning of the value is as follows:

-   true: Encryption is allowed.
-   flase: Encryption is not allowed. |
|diskType|String|cloud\_ssd|The storage type of the disk. |
|maxSize|Integer|5120|The maximum value allowed for a disk. |
|minSize|Integer|500|The minimum value allowed for a disk. |
|scaleLimit|Integer|2048|The disk allows setting the maximum value of consecutive values. |
|valueLimitSet|List|\[2560,3072,3584,4096,4608,5120\]|The discrete value allowed by the disk. |
|spec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.8xlarge","elasticsearch.ic5.large","elasticsearch.ic5.xlarge","elasticsearch.ic5.2xlarge","elasticsearch.ic5.3xlarge","elasticsearch.ic5.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge"\]|The node specification. |
|env|String|production|Environmental signs. |
|esVersions|List|\[ "5.5.3\_with\_X-Pack"\]|The list of Elasticsearch versions allowed to be created. |
|esVersionsLatestList|Array of esVersionsLatestList| |The ES version is open for sale. |
|key|String|5.5\_with\_X-Pack|Supported major version numbers. |
|value|String|5.5.3\_with\_X-Pack|The full name of the supported minor version number. |
|instanceSupportNodes|List|\[ "WORKER", "WORKER\_WARM", "COORDINATING", "KIBANA", "MASTER", "ELASTIC\_WORKER" \]|The instance node type that is open in the region. |
|jvmConfine|Struct| |Jvm verifies the configuration. |
|memory|Integer|32|Turn on the minimum memory value of the specifications required for Jvm recycling. |
|supportEsVersions|List|\["6.7.0\_with\_X-Pack","6.7.0\_with\_A-Pack","7.4.0\_with\_X-Pack"\]|Enable the ES version information supported by Jvm recycling. |
|supportGcs|List|\["CMS","G1"\]|The list of Jvm recyclers that are allowed to be set. |
|kibanaNodeProperties|Struct| |The configuration of Kibana nodes. |
|amountRange|Struct| |The allowable value range for the number of nodes. |
|maxAmount|Integer|20|The maximum number of nodes. |
|minAmount|Integer|1|The minimum number of nodes. |
|spec|List|\["elasticsearch.n4.small","elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn1ne.large"\]|The list of specifications that are allowed to be set. |
|masterDiskList|Array of disk| |Exclusive master node disk allowable value. |
|diskType|String|cloud\_ssd|The storage type of the disk. |
|maxSize|Integer|20|The maximum value allowed for a disk. |
|minSize|Integer|20|The minimum value allowed for a disk. |
|scaleLimit|Integer|20|The disk allows setting the maximum value of consecutive values. |
|masterSpec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge" \]|The exclusive master node specifications. |
|node|Struct| |The node configuration. |
|maxAmount|Integer|50|The maximum number of nodes allowed for a data node. |
|minAmount|Integer|2|The minimum number of nodes allowed for a data node. |
|nodeSpecList|Array of nodeSpecList| |The list of data node specifications. |
|cpuCount|Integer|16|The number of CPU cores corresponding to this specification. |
|disk|Integer|44000|The size of the disk corresponding to the specification. |
|diskType|String|local\_efficiency|The storage type of the disk. |
|enable|Boolean|true|Whether it can be purchased: true \(can be purchased\), flase \(cannot be purchased\). |
|memorySize|Integer|64|The node memory size. |
|spec|String|elasticsearch.sn2ne.large|The name of the specification. |
|specGroupType|String|local\_efficiency|The storage type, which supports the following three types:

-   common: cloud disk type
-   local\_efficiency: local SATA disk.
-   local\_ssd: the local SSD disk. |
|regionId|String|cn-hangzhou|The current region ID. |
|supportVersions|Array of CategoryEntity| |Supported version configurations. |
|instanceCategory|String|x-pack|The instance category, which supports the following two categories:

-   advanced enhanced edition.
-   x-pack commercial edition. |
|supportVersionList|Array of VersionEntity| |The information of supported Elsticsearch versions. |
|key|String|5.5|The sale page supports optional versions. |
|value|String|5.5.3|The detailed version number. |
|warmNodeProperties|Struct| |Cold node configuration. |
|amountRange|Struct| |The node number range value. |
|maxAmount|Integer|50|The maximum number of nodes. |
|minAmount|Integer|2|The minimum number of nodes. |
|diskList|Array of disk| |The list of disk configurations. |
|diskEncryption|Boolean|true|The disk allows encryption. |
|diskType|String|cloud\_efficiency|The storage type of the disk. |
|maxSize|Integer|5120|The maximum value allowed for a disk. |
|minSize|Integer|500|The minimum value allowed for a disk. |
|scaleLimit|Integer|2048|The disk allows setting the maximum value of consecutive values. |
|valueLimitSet|List|\[2560,3072,3584,4096,4608,5120\]|The discrete value allowed by the disk. |
|spec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.8xlarge","elasticsearch.ic5.large","elasticsearch.ic5.xlarge","elasticsearch.ic5.2xlarge","elasticsearch.ic5.3xlarge","elasticsearch.ic5.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge"\]|The specifications supported by cold nodes. |
|zones|List|\["cn-hangzhou-b","cn-hangzhou-f"\]|Supported zones. |

**Note:** The above sample values are for reference only, subject to the actual return value of the interface.

## Examples

Sample requests

```

     GET /openapi/region 
   
```

Sample success responses

`JSON` format

```

     { "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6F******", "Result": { "regionId": "cn-hangzhou", "createUrl": "https://common-buy.aliyun.com/?commodityCode=elasticsearch&amp;orderType=BUY#/buy", "env": "production", "dataDiskList": { "scaleLimit": 2048, "maxSize": 5120, "minSize": 20, "diskType": "cloud_ssd", "valueLimitSet": "[2560,3072,3584,4096,4608,5120]" }, "supportVersions": { "instanceCategory": "x-pack", "supportVersionList": { "value": "5.5.3", "key": 5.5 } }, "nodeSpecList": { "disk": 44000, "memorySize": 64, "enable": true, "diskType": "local_efficiency", "specGroupType": "local_efficiency", "spec": "elasticsearch.sn2ne.large", "cpuCount": 16 }, "clientNodeDiskList": { "scaleLimit": 18, "maxSize": 20, "minSize": 20, "diskType": "cloud_efficiency" }, "esVersionsLatestList": { "value": "5.5.3_with_X-Pack", "key": "5.5_with_X-Pack" }, "masterDiskList": { "scaleLimit": 20, "maxSize": 20, "minSize": 20, "diskType": "cloud_ssd" }, "zones": "[\"cn-hangzhou-b\",\"cn-hangzhou-f\"]", "esVersions": "[ \"5.5.3_with_X-Pack\"]", "masterSpec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\" ]", "clientNodeSpec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.4xlarge\",\"elasticsearch.ic5.xlarge\",\"elasticsearch.ic5.2xlarge\",\"elasticsearch.ic5.3xlarge\",\"elasticsearch.ic5.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\"]", "instanceSupportNodes": "[ \"WORKER\", \"WORKER_WARM\", \"COORDINATING\", \"KIBANA\", \"MASTER\", \"ELASTIC_WORKER\" ]", "node": { "minAmount": 2, "maxAmount": 50 }, "jvmConfine": { "memory": 32, "supportGcs": "[\"CMS\",\"G1\"]", "supportEsVersions": "[\"6.7.0_with_X-Pack\",\"6.7.0_with_A-Pack\",\"7.4.0_with_X-Pack\"]" }, "clientNodeAmountRange": { "minAmount": 2, "maxAmount": 25 }, "warmNodeProperties": { "diskList": { "scaleLimit": 2048, "minSize": 500, "maxSize": 5120, "diskType": "cloud_efficiency", "diskEncryption": true, "valueLimitSet": "[2560,3072,3584,4096,4608,5120]" }, "spec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.8xlarge\",\"elasticsearch.ic5.large\",\"elasticsearch.ic5.xlarge\",\"elasticsearch.ic5.2xlarge\",\"elasticsearch.ic5.3xlarge\",\"elasticsearch.ic5.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\"]", "amountRange": { "minAmount": 2, "maxAmount": 50 } }, "kibanaNodeProperties": { "spec": "[\"elasticsearch.n4.small\",\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn1ne.large\"]", "amountRange": { "minAmount": 1, "maxAmount": 20 } }, "elasticNodeProperties": { "diskList": { "scaleLimit": 2048, "minSize": 500, "maxSize": 5120, "diskType": "cloud_ssd", "diskEncryption": true, "valueLimitSet": "[2560,3072,3584,4096,4608,5120]" }, "spec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.8xlarge\",\"elasticsearch.ic5.large\",\"elasticsearch.ic5.xlarge\",\"elasticsearch.ic5.2xlarge\",\"elasticsearch.ic5.3xlarge\",\"elasticsearch.ic5.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\"]", "amountRange": { "minAmount": 2, "maxAmount": 25 } } } } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

