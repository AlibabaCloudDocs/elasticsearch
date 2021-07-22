# DescribeInstance

Call DescribeInstance to query detailed information about a specified Elasticsearch instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeInstance&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId] HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-s9dsk3k4k\*\*\*\*|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|advancedDedicateMaster|Boolean|true|Indicates whether the instance contains dedicated master nodes. |
|advancedSetting|Struct| |Advanced Configuration. |
|gcName|String|CMS|The name of the GC garbage collector. CMS and G1 are supported. |
|aliwsDicts|Array of Dict| |The configuration of the Alibaba word segmentation Dictionary. |
|fileSize|Long|2782602|The size of the Dictionary File. Unit: bytes. |
|name|String|aliws\_ext\_dict.txt|The name of the dictionary file. |
|sourceType|String|OSS|The type of the Dictionary File Source. Valid values:

-   OSS:OSS open storage \(make sure the OSS bucket is publicly readable.\)
-   ORIGIN: open-source Elasticsearch
-   UPLOAD: uploaded files |
|type|String|ALI\_WS|The type of the Dictionary File. Valid values:

-   STOP: stopwords
-   MAIN: MAIN Dictionary
-   SYNONYMS: Synonym Dictionary
-   ALI\_WS: Alibaba Dictionary |
|clientNodeConfiguration|Struct| |The configuration information of the coordination node. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|40|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_efficiency|The storage type of the node. Only ultra disks \(cloud\_efficiency\) are supported. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|createdAt|String|2018-07-13T03:58:07.253Z|The time when the instance was created. |
|dedicateMaster|Boolean|false|Indicates whether the instance contains dedicated master nodes. This parameter is only supported by earlier Elasticsearch versions. |
|description|String|es-cn-abc|The name of the instance. |
|dictList|Array of DictList| |The configuration of the IK dictionary. |
|fileSize|Long|2782602|The size of the Dictionary File. Unit: bytes. |
|name|String|SYSTEM\_MAIN.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The type of the Dictionary File Source. Valid values:

-   OSS:OSS open storage \(make sure the OSS bucket is publicly readable.\)
-   ORIGIN: open-source Elasticsearch
-   UPLOAD: uploaded files |
|type|String|MAIN|The type of the Dictionary File. Valid values:

-   STOP: stopwords
-   MAIN: MAIN Dictionary
-   SYNONYMS: Synonym Dictionary
-   ALI\_WS: Alibaba Dictionary |
|domain|String|es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com|The internal network address of the instance. |
|elasticDataNodeConfiguration|Struct| |The configuration of the elastic data node. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskEncryption|Boolean|true|Specifies whether to enable cloud disk encryption for the node. |
|diskType|String|cloud\_ssd|The storage type of the node. Supported:

-   cloud\_ssd: standard SSD
-   cloud\_essd: enhanced SSD \(ESSD\)
-   cloud\_efficiency: ultra disk |
|spec|String|elasticsearch.sn2ne.large|The node specification. |
|enableKibanaPrivateNetwork|Boolean|false|Whether to enable Kibana private network access. |
|enableKibanaPublicNetwork|Boolean|true|Specifies whether to enable Kibana for public network access. |
|enablePublic|Boolean|true|Specifies whether to enable the public network access feature for the cluster. |
|esConfig|Map|\{"http.cors.allow-credentials":"false"\}|The YML configuration file of the instance. |
|esIPBlacklist|List|\[ "0.0.0.0/0" \]|The private network access blacklist \(abandoned\). |
|esIPWhitelist|List|\[ "0.0.0.0/0" \]|The private network access whitelist \(discarded\). |
|esVersion|String|5.5.3\_with\_X-Pack|The version of the instance. |
|extendConfigs|List|\[\{ "configType": "aliVersion","aliVersion": "ali1.3.0" \}\]|The extended configurations of the instance. |
|haveClientNode|Boolean|true|Indicates whether the instance contains client nodes. |
|haveKibana|Boolean|true|Indicates whether the instance contains Kibana nodes. |
|instanceId|String|es-cn-abc|The ID of the Elasticsearch instance. |
|kibanaConfiguration|Struct| |The configuration of Kibana nodes. |
|amount|Integer|1|The number of DRDS server nodes. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|kibanaDomain|String|es-cn-abc.kibana.elasticsearch.aliyuncs.com|The endpoint of Kibana. |
|kibanaIPWhitelist|List|\[ "0.0.0.0/0" \]|Kibana public endpoint access whitelist. |
|kibanaPort|Integer|5601|The port used to access Kibana. |
|kibanaPrivateIPWhitelist|List|\["192.168.xx.xx"\]|The list of Kibana private address access whitelists. |
|masterConfiguration|Struct| |The configuration of dedicated master nodes. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|40|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. Only cloud\_ssd is supported. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|The type of the network. Only Virtual Private Cloud \(VPC\) is supported. |
|vpcId|String|vpc-abc|The ID of the VPC network. |
|vsArea|String|cn-hangzhou-b|The zone where the instance is deployed. |
|vswitchId|String|vsw-abc|The ID of the vSwitch in the specified VPC. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration of data nodes. |
|disk|Integer|0|The storage space of the node. Unit: GB. |
|diskEncryption|Boolean|true|Indicates whether to enable disk encryption. Valid values:

-   true: Dynamic indexing is enabled.
-   false: Dynamic indexing is disabled. |
|diskType|String|cloud\_ssd|The disk type of the node. Supported options: cloud\_ssd\(SSD cloud disks\) and cloud\_efficiency \(ultra cloud disks\). |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|paymentType|String|postpaid|The billing method of the physical connection. Supported: **prepaid** \(Subscription\) and **postpaid** \(Pay-as-you-go\). |
|port|Integer|9200|The port number that is used to access your Elasticsearch cluster. |
|privateNetworkIpWhiteList|List|0.0.0.0/0|The whitelist for access to the private network addresses of the instance. |
|protocol|String|HTTP|The communication protocol. Supported: **HTTP** and **HTTPS**. |
|publicDomain|String|es-cn-abc.elasticsearch.aliyuncs.com|The public endpoint of the instance. |
|publicIpWhitelist|List|\[ "0.0.0.0/0" \]|The public endpoint access whitelist of the instance. |
|publicPort|Integer|9200|The public network access port of the instance. |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|The ID of the resource group to which the ApsaraDB for Redis instance belongs. |
|status|String|active|The status of the cluster. Supported: **active** \(Normal\), **activating** \(In effect\), **inactive** \(Frozen\) and **invalid** \(Invalidation\). |
|synonymsDicts|Array of SynonymsDicts| |The configuration of the synonym dictionary. |
|fileSize|Long|2782602|The size of the Dictionary File. Unit: bytes. |
|name|String|SYSTEM\_MAIN.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source of the synonym dictionary file. |
|type|String|STOP|The type of the synonym dictionary. STOP: stopwords, MAIN: MAIN dictionary, SYNONYMS: Synonym Dictionary, and ALI\_WS: Alibaba Dictionary. |
|tags|Array of Tag| |Details about the tags. |
|tagKey|String|env|The tag key of the disk. |
|tagValue|String|dev|The tag value of the disk. |
|updatedAt|String|2018-07-13T03:58:07.253Z|The time when the instance was last updated. |
|vpcInstanceId|String|vpc-bp1uag5jj38c\*\*\*\*|The ID of the VPC. |
|warmNode|Boolean|true|Specifies whether to enable cold data nodes. |
|warmNodeConfiguration|Struct| |The configuration information of cold data nodes. |
|amount|Integer|6|The number of nodes. |
|disk|Integer|500|The storage space of the node. Unit: GB. |
|diskEncryption|Boolean|true|Whether to enable cloud disk encryption. |
|diskType|String|cloud\_efficiency|The storage space type of the node. Only supports cloud\_efficiency \(ultra disks\). |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|zoneCount|Integer|2|The number of zones. |
|zoneInfos|Array of ZoneInfo| |Zone information. |
|status|String|NORMAL|The status of the zone. Supported: **ISOLATION**\(Offline\), **NORMAL** \(Normal\). |
|zoneId|String|cn-hangzhou-b|The ID of the zone. |

**Note:** In the following return example, only the parameters in the returned data list are guaranteed to be included in this article, and the parameters that are not mentioned are for reference only. It is not mandatory to obtain these parameters in the program.

## Examples

Sample requests

```
GET /openapi/instances/es-cn-s9dsk3k4k**** HTTP/1.1 
common request header
```

Sample success responses

`XML` format

```
<Result>
    <instanceId>es-cn-n6w1ptcb30009****</instanceId>
    <version>5.5.3_with_X-Pack</version>
    <description>es-cn-n6w1ptcb30009****</description>
    <nodeAmount>3</nodeAmount>
    <paymentType>postpaid</paymentType>
    <status>active</status>
    <privateNetworkIpWhiteList>0.0.0.0/0</privateNetworkIpWhiteList>
    <enablePublic>true</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.n4.small</spec>
        <disk>20</disk>
        <diskType>cloud_ssd</diskType>
        <diskEncryption>false</diskEncryption>
    </nodeSpec>
    <networkConfig>
        <vpcId>vpc-bp16k1dvzxtmagcva****</vpcId>
        <vswitchId>vsw-bp1k4ec6s7sjdbudw****</vswitchId>
        <vsArea>cn-hangzhou-i</vsArea>
        <type>vpc</type>
    </networkConfig>
    <createdAt>2020-06-28T08:25:52.895Z</createdAt>
    <updatedAt>2020-06-28T08:25:52.895Z</updatedAt>
    <commodityCode>elasticsearch</commodityCode>
    <extendConfigs>
        <configType>usageScenario</configType>
        <value>general</value>
    </extendConfigs>
    <extendConfigs>
        <configType>maintainTime</configType>
        <maintainStartTime>02:00Z</maintainStartTime>
        <maintainEndTime>06:00Z</maintainEndTime>
    </extendConfigs>
    <endTime>4749033600000</endTime>
    <vpcInstanceId>es-cn-n6w1ptcb30009****-worker</vpcInstanceId>
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-hangzhou-i</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>elasticsearch</instanceType>
    <inited>true</inited>
    <domain>es-cn-n6w1ptcb30009****.elasticsearch.aliyuncs.com</domain>
    <port>9200</port>
    <esVersion>5.5.3_with_X-Pack</esVersion>
    <esConfig>
        <action.destructive_requires_name>true</action.destructive_requires_name>
        <xpack.security.audit.outputs>index</xpack.security.audit.outputs>
        <xpack.watcher.enabled>false</xpack.watcher.enabled>
        <xpack.security.audit.enabled>false</xpack.security.audit.enabled>
        <action.auto_create_index>true</action.auto_create_index>
    </esConfig>
    <esIPWhitelist>0.0.0.0/0</esIPWhitelist>
    <kibanaIPWhitelist>0.0.0.0/0</kibanaIPWhitelist>
    <kibanaIPWhitelist>::/0</kibanaIPWhitelist>
    <publicIpWhitelist>::1</publicIpWhitelist>
    <publicIpWhitelist>0.0.0.0/0</publicIpWhitelist>
    <kibanaDomain>es-cn-n6w1ptcb30009****.kibana.elasticsearch.aliyuncs.com</kibanaDomain>
    <kibanaPort>5601</kibanaPort>
    <publicPort>9200</publicPort>
    <publicDomain>es-cn-n6w1ptcb30009****.public.elasticsearch.aliyuncs.com</publicDomain>
    <haveKibana>true</haveKibana>
    <instanceCategory>x-pack</instanceCategory>
    <dedicateMaster>false</dedicateMaster>
    <advancedDedicateMaster>false</advancedDedicateMaster>
    <masterConfiguration/>
    <haveClientNode>false</haveClientNode>
    <warmNode>false</warmNode>
    <warmNodeConfiguration/>
    <clientNodeConfiguration/>
    <kibanaConfiguration>
        <spec>elasticsearch.n4.small</spec>
        <amount>1</amount>
        <disk>0</disk>
    </kibanaConfiguration>
    <dictList>
        <name>SYSTEM_MAIN.dic</name>
        <fileSize>3058510</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>MAIN</type>
    </dictList>
    <dictList>
        <name>SYSTEM_STOPWORD.dic</name>
        <fileSize>164</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>STOP</type>
    </dictList>
    <haveGrafana>false</haveGrafana>
    <haveCerebro>false</haveCerebro>
    <enableKibanaPublicNetwork>true</enableKibanaPublicNetwork>
    <enableKibanaPrivateNetwork>false</enableKibanaPrivateNetwork>
    <advancedSetting>
        <gcName>CMS</gcName>
    </advancedSetting>
</Result>
<RequestId>D6888749-44DB-4510-A5DF-2959A035****</RequestId>
```

`JSON` Syntax

```
{
    "Result": {
        "instanceId": "es-cn-n6w1ptcb30009****",
        "version": "5.5.3_with_X-Pack",
        "description": "es-cn-n6w1ptcb30009****",
        "nodeAmount": 3,
        "paymentType": "postpaid",
        "status": "active",
        "privateNetworkIpWhiteList": [
            "0.0.0.0/0"
        ],
        "enablePublic": true,
        "nodeSpec": {
            "spec": "elasticsearch.n4.small",
            "disk": 20,
            "diskType": "cloud_ssd",
            "diskEncryption": false
        },
        "networkConfig": {
            "vpcId": "vpc-bp16k1dvzxtmagcva****",
            "vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
            "vsArea": "cn-hangzhou-i",
            "type": "vpc"
        },
        "createdAt": "2020-06-28T08:25:52.895Z",
        "updatedAt": "2020-06-28T08:25:52.895Z",
        "commodityCode": "elasticsearch",
        "extendConfigs": [
            {
                "configType": "usageScenario",
                "value": "general"
            },
            {
                "configType": "maintainTime",
                "maintainStartTime": "02:00Z",
                "maintainEndTime": "06:00Z"
            }
        ],
        "endTime": 4749033600000,
        "clusterTasks": [],
        "vpcInstanceId": "es-cn-n6w1ptcb30009****-worker",
        "resourceGroupId": "rg-acfm2h5vbzd****",
        "zoneCount": 1,
        "protocol": "HTTP",
        "zoneInfos": [
            {
                "zoneId": "cn-hangzhou-i",
                "status": "NORMAL"
            }
        ],
        "instanceType": "elasticsearch",
        "inited": true,
        "tags": [],
        "domain": "es-cn-n6w1ptcb30009****.elasticsearch.aliyuncs.com",
        "port": 9200,
        "esVersion": "5.5.3_with_X-Pack",
        "esConfig": {
            "action.destructive_requires_name": "true",
            "xpack.security.audit.outputs": "index",
            "xpack.watcher.enabled": "false",
            "xpack.security.audit.enabled": "false",
            "action.auto_create_index": "true"
        },
        "esIPWhitelist": [
            "0.0.0.0/0"
        ],
        "esIPBlacklist": [],
        "kibanaIPWhitelist": [
            "0.0.0.0/0",
            "::/0"
        ],
        "kibanaPrivateIPWhitelist": [],
        "publicIpWhitelist": [
            "::1",
            "0.0.0.0/0"
        ],
        "kibanaDomain": "es-cn-n6w1ptcb30009****.kibana.elasticsearch.aliyuncs.com",
        "kibanaPort": 5601,
        "publicPort": 9200,
        "publicDomain": "es-cn-n6w1ptcb30009****.public.elasticsearch.aliyuncs.com",
        "haveKibana": true,
        "instanceCategory": "x-pack",
        "dedicateMaster": false,
        "advancedDedicateMaster": false,
        "masterConfiguration": {},
        "haveClientNode": false,
        "warmNode": false,
        "warmNodeConfiguration": {},
        "clientNodeConfiguration": {},
        "kibanaConfiguration": {
            "spec": "elasticsearch.n4.small",
            "amount": 1,
            "disk": 0
        },
        "dictList": [
            {
                "name": "SYSTEM_MAIN.dic",
                "fileSize": 3058510,
                "sourceType": "ORIGIN",
                "type": "MAIN"
            },
            {
                "name": "SYSTEM_STOPWORD.dic",
                "fileSize": 164,
                "sourceType": "ORIGIN",
                "type": "STOP"
            }
        ],
        "synonymsDicts": [],
        "ikHotDicts": [],
        "aliwsDicts": [],
        "haveGrafana": false,
        "haveCerebro": false,
        "enableKibanaPublicNetwork": true,
        "enableKibanaPrivateNetwork": false,
        "advancedSetting": {
            "gcName": "CMS"
        }
    },
    "RequestId": "D6888749-44DB-4510-A5DF-2959A035****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

