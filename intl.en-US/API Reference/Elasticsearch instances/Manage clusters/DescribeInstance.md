# DescribeInstance

You can call this operation to query detailed information about a specified Elasticsearch instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeInstance&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances/[InstanceId] HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-3h4k3axh33th9\*\*\*\*|The ID of the DRDS instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Result|Struct| |The returned results. |
|advancedDedicateMaster|Boolean|true|Indicates whether the instance contains dedicated master nodes. The values are as follows:

-   true: Contains dedicated master nodes.
-   false: Not contains dedicated master nodes. |
|advancedSetting|Struct| |Advanced configuration. |
|gcName|String|CMS|The name of the GC garbage collector. CMS and G1. |
|aliwsDicts|Array of Dict| |The configuration of the alibaba cloud dictionary. |
|fileSize|Long|2782602|The size of the dictionary file. Unit: bytes. |
|name|String|aliws\_ext\_dict.txt|The name of the dictionary file. |
|sourceType|String|OSS|The source type of the dictionary file. Valid values:

-   OSS: OSS is open for storage \(you need to ensure that the OSS bucket is publicly readable\).
-   ORIGIN: the open source Elasticsearch.
-   UPLOAD: the uploaded file. |
|type|String|ALI\_WS|The type of the dictionary file. Valid values:

-   STOP: stop words.
-   MAIN: the main dictionary.
-   SYNONYMS: synonym dictionary.
-   ALI\_WS: Alibaba Dictionary. |
|clientNodeConfiguration|Struct| |The configuration information of the coordination node. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|40|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_efficiency|The storage type of the node. Only ultra disks \(cloud\_efficiency\) are supported. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. Specification information is available through [Product Specifications](~~271718~~) View. |
|createdAt|String|2018-07-13T03:58:07.253Z|The time when the instance was created. |
|dedicateMaster|Boolean|false|Dedicated master node \(obsolete\). |
|description|String|es-cn-abc|The name of the instance. |
|dictList|Array of DictList| |The configuration of the IK dictionary. |
|fileSize|Long|2782602|The size of the dictionary file. Unit: bytes. |
|name|String|SYSTEM\_MAIN.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source type of the dictionary file. Valid values:

-   OSS: OSS is open for storage \(you need to ensure that the OSS bucket is publicly readable\).
-   ORIGIN: the open source Elasticsearch.
-   UPLOAD: the uploaded file. |
|type|String|MAIN|The type of the dictionary file. Valid values:

-   STOP: stop words.
-   MAIN: the main dictionary.
-   SYNONYMS: synonym dictionary.
-   ALI\_WS: Alibaba Dictionary. |
|domain|String|es-cn-3h4k3axh33th9\*\*\*\*.elasticsearch.aliyuncs.com|The internal endpoint of the instance. |
|elasticDataNodeConfiguration|Struct| |The configuration of the elastic data node. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskEncryption|Boolean|true|Specifies whether to enable disk encryption for the node. Valid values:

-   true: enables MFA
-   flase: disabled. |
|diskType|String|cloud\_ssd|The storage type of the node. Valid values:

-   cloud\_ssd: standard SSD.
-   cloud\_essd: enhanced SSD
-   cloud\_efficiency: ultra disk |
|spec|String|elasticsearch.sn2ne.large|The node specification. Specification information is available through [Product Specifications](~~271718~~) View. |
|enableKibanaPrivateNetwork|Boolean|false|Specifies whether to enable Kibana private network access. The values are as follows:

-   true: BFD is enabled.
-   false: BFD is disabled. |
|enableKibanaPublicNetwork|Boolean|true|Specifies whether to enable Internet access to Kibana. The values are as follows:

-   true: BFD is enabled.
-   false: BFD is disabled. |
|enablePublic|Boolean|true|Specifies whether to enable the public network access feature for the Elasticsearch cluster. The values are as follows:

-   true: BFD is enabled.
-   false: BFD is disabled. |
|esConfig|Map|\{"http.cors.allow-credentials":"false"\}|The YML file configuration information of the instance. |
|esIPBlacklist|List|\[ "0.0.0.0/0" \]|The private network access blacklist \(obsolete\). |
|esIPWhitelist|List|\[ "0.0.0.0/0" \]|The private network access whitelist \(obsolete\). |
|esVersion|String|6.3.2\_with\_X-Pack|The version of the instance. |
|extendConfigs|List|\[\{ "configType": "aliVersion","aliVersion": "ali1.3.0" \}\]|The extended configurations of the instance. |
|haveClientNode|Boolean|true|Indicates whether the instance contains client nodes. The values are as follows:

-   true: Contains client nodes.
-   false: Not contains client nodes. |
|haveKibana|Boolean|true|Indicates whether the instance contains Kibana nodes. The values are as follows:

-   true: Contains Kibana nodes.
-   false: Not contains Kibana nodes. |
|instanceId|String|es-cn-3h4k3axh33th9\*\*\*\*|The ID of the instance. |
|isNewDeployment|Boolean|true|Specifies whether to deploy the new architecture. |
|kibanaConfiguration|Struct| |The configuration of Kibana nodes. |
|amount|Integer|1|The number of DRDS server nodes. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. Specification information is available through [Product Specifications](~~271718~~) View. |
|kibanaDomain|String|es-cn-3h4k3axh33th9\*\*\*\*.kibana.elasticsearch.aliyuncs.com|The endpoint of Kibana. |
|kibanaIPWhitelist|List|\[ "0.0.0.0/0" \]|The list of Kibana public endpoint access whitelists. |
|kibanaPort|Integer|5601|The access port of Kibana. |
|kibanaPrivateIPWhitelist|List|\["192.168.XX.XX"\]|The whitelist of Kibana private endpoint. |
|masterConfiguration|Struct| |The configuration of dedicated master nodes. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|40|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. Only cloud\_ssd\(SSD\) is supported. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. Specification information is available through [Product Specifications](~~271718~~) View. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|The network type. Only Virtual Private Cloud \(VPC\) is supported. |
|vpcId|String|vpc-abc|The ID of the VPC network. |
|vsArea|String|cn-hangzhou-b|The zone where the instance is deployed. |
|vswitchId|String|vsw-abc|The IDs of vSwitches. |
|whiteIpGroupList|Array of whiteIpGroupList| |The list of whitelists. |
|groupName|String|default|The group name of the whitelist group. The default group is included by default. |
|ips|List|\["0.0.0.0", "127.0.XX.XX"\]|The list of IP addresses in the whitelist group. |
|whiteIpType|String|PRIVATE\_ES|The type of the whitelist. The values are as follows:

-   PRIVATE\_ES: Elasticsearch the private network.
-   PUBLIC\_ES: Elasticsearch the Internet.
-   PRIVATE\_KIBANA: the Kibana private network.
-   PUBLIC\_KIBANA: the public network of Kibana. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration of data nodes. |
|disk|Integer|0|The storage space of the node. Unit: GB. |
|diskEncryption|Boolean|true|Specifies whether to enable disk encryption. Valid values:

-   true: BFD is enabled.
-   false: BFD is disabled. |
|diskType|String|cloud\_ssd|The disk type of the node. Supported: cloud\_ssd\(SSD\) and cloud\_efficiency \(ultra disk\). |
|spec|String|elasticsearch.n4.small|The specification of data nodes. Specification information is available through [Product Specifications](~~271718~~) View. |
|paymentType|String|postpaid|The billing method of the instance. Valid values:

-   prepaid: subscription.
-   postpaid: pay-as-you-go. |
|port|Integer|9200|The port number that is used to access your Elasticsearch cluster. |
|postpaidServiceStatus|String|active|The status of the pay-as-you-go service overlaid on the subscription instance. The values are as follows:

-   active: normal.
-   closed: disabled.
-   indebt: The overdue payment is being frozen. |
|privateNetworkIpWhiteList|List|0.0.0.0/0|The whitelist of the private endpoint of the instance. |
|protocol|String|HTTP|The communication protocol. Supported: HTTP and HTTPS. |
|publicDomain|String|es-cn-3h4k3axh33th9\*\*\*\*.elasticsearch.aliyuncs.com|The public endpoint of the instance. |
|publicIpWhitelist|List|\[ "0.0.0.0/0" \]|The public endpoint access whitelist of the instance. |
|publicPort|Integer|9200|The public access port of the instance. |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|The ID of the resource group to which the ApsaraDB for Redis instance belongs. |
|serviceVpc|Boolean|true|whether it is a service vpc. |
|status|String|active|The status of the instance. Valid values:

-   active: normal.
-   activating: The instance is taking effect.
-   inactive: freezes.
-   invalid: invalid. |
|synonymsDicts|Array of SynonymsDicts| |The configuration of the synonym dictionary. |
|fileSize|Long|2782602|The size of the dictionary file. Unit: bytes. |
|name|String|SYSTEM\_MAIN.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source of the synonym dictionary file. |
|type|String|STOP|The type of the synonym dictionary. The values are as follows:

-   STOP: stop words.
-   MAIN: the main dictionary.
-   SYNONYMS: synonym dictionary.
-   ALI\_WS: Alibaba Dictionary. |
|tags|Array of Tag| |Details about the tags. |
|tagKey|String|env|The key of tag N. |
|tagValue|String|dev|The value of the label. |
|updatedAt|String|2018-07-13T03:58:07.253Z|The time when the instance was last updated. |
|vpcInstanceId|String|vpc-bp1uag5jj38c\*\*\*\*|The ID of the virtual private cloud \(VPC\). |
|warmNode|Boolean|true|Specifies whether to enable the cold data node. The values are as follows:

-   true: BFD is enabled.
-   false: BFD is disabled. |
|warmNodeConfiguration|Struct| |The configuration information of the cold data node. |
|amount|Integer|6|The number of nodes. |
|disk|Integer|500|The storage space of the node. Unit: GB. |
|diskEncryption|Boolean|true|Specifies whether to enable disk encryption. The values are as follows:

-   true: BFD is enabled.
-   false: BFD is disabled. |
|diskType|String|cloud\_efficiency|The storage type of the node. Only cloud\_efficiency \(ultra disks\) are supported. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. Specification information is available through [Product Specifications](~~271718~~) View. |
|zoneCount|Integer|2|The number of zones. |
|zoneInfos|Array of ZoneInfo| |Zone information. |
|status|String|NORMAL|The status of the zone. Supported: ISOLATION and NORMAL. |
|zoneId|String|cn-hangzhou-b|The ID of the zone to which the resources belong. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |

**Note:** In the following return example, this article only guarantees that the parameters in the return data list are included, and the parameters not mentioned are for reference only. The program cannot force to rely on obtaining these parameters.

## Examples

Sample requests

```

     GET /openapi/instances/es-cn-3h4k3axh33th9**** HTTP/1.1 common request headers 
   
```

Sample success responses

`JSON` format

```

     { "Result": { "instanceId": "es-cn-3h4k3axh33th9****", "domain": "es-cn-3h4k3axh33th9****.elasticsearch.aliyuncs.com", "description": "es-cn-3h4k3axh33th9****", "nodeAmount": 2, "paymentType": "postpaid", "status": "active", "port": 9200, "esVersion": "6.3.2_with_X-Pack", "instanceCategory": "x-pack", "esConfig": { "action.destructive_requires_name": "true", "xpack.security.audit.outputs": "index", "xpack.watcher.enabled": "false", "xpack.security.audit.enabled": "false", "action.auto_create_index": "+.*,-*" }, "esIPWhitelist": [ "0.0.0.0/0" ], "esIPBlacklist": [], "privateNetworkIpWhiteList": [ "0.0.0.0/0" ], "kibanaIPWhitelist": [ "0.0.0.0/0", "::/0" ], "publicIpWhitelist": [], "kibanaDomain": "es-cn-3h4k3axh33th9****.kibana.elasticsearch.aliyuncs.com", "kibanaPort": 5601, "enablePublic": false, "haveKibana": true, "nodeSpec": { "spec": "elasticsearch.sn1ne.large", "disk": 21, "diskType": "cloud_ssd" }, "serviceVpc": true, "networkConfig": { "vpcId": "vpc-bp13n1j3fcv1cqfkg****", "vswitchId": "vsw-bp12q2vrqvko0ilg8****", "vsArea": "cn-hangzhou-h", "type": "vpc", "whiteIpGroupList": [ { "groupName": "default", "whiteIpType": "PRIVATE_ES", "ips": [ "0.0.0.0", "127.0.XX.XX" ] } ] }, "createdAt": "2019-08-28T07:48:59.736Z", "updatedAt": "2019-08-28T08:11:33.229Z", "inited": true, "dedicateMaster": false, "advancedDedicateMaster": false, "masterConfiguration": {}, "haveClientNode": true, "warmNode": true, "warmNodeConfiguration": { "spec": "elasticsearch.ic5.large", "amount": 2, "diskType": "cloud_efficiency", "disk": 500, "diskEncryption": true }, "clientNodeConfiguration": { "spec": "elasticsearch.ic5.large", "amount": 2, "diskType": "cloud_efficiency", "disk": 20 }, "kibanaConfiguration": { "spec": "elasticsearch.n4.small", "amount": 1, "disk": 0 }, "commodityCode": "elasticsearch", "endTime": 4722681600000, "dictList": [ { "name": "SYSTEM_MAIN.dic", "fileSize": 2782602, "type": "MAIN", "sourceType": "ORIGIN" }, { "name": "SYSTEM_STOPWORD.dic", "fileSize": 132, "type": "STOP", "sourceType": "ORIGIN" } ], "synonymsDicts": [], "ikHotDicts": [], "aliwsDicts": [], "clusterTasks": [], "vpcInstanceId": "es-cn-3h4k3axh33th9****-worker", "resourceGroupId": "rg-acfm3ghp32r****", "zoneCount": 1, "protocol": "HTTP", "haveGrafana": false, "haveCerebro": false, "zoneInfos": [ { "zoneId": "cn-hangzhou-h", "status": "NORMAL" } ], "enableKibanaPublicNetwork": true, "advancedSetting": { "gcName": "CMS" } }, "RequestId": "68104CC2-DA27-446D-8049-A024F0D4EFEC" } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

