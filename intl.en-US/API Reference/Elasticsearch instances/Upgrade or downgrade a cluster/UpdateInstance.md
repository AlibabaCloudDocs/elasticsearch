# UpdateInstance

You can call the UpdateInstance operation to change the configuration of a cluster. The configuration can be upgraded or downgraded.

When you call this operation, take note of the following limits:

-   You cannot change the configuration of an instance when it is in the activating, invalid, or inactive state.
-   You can only change the configuration of one type of node at a time, including data nodes, dedicated master nodes, cold data nodes, coordinator nodes, Kibana nodes, and elastic nodes. For more information, see [Upgrade the configuration of a cluster](~~96650~~) and [Downgrade cluster](~~198887~~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateInstance&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
PUT /openapi/instances/[InstanceId] HTTP/1.1
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1ptcb30009\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|ignoreStatus|Boolean|Query|No|true|Indicates whether to ignore the cluster status when you change the configuration of the cluster. Valid values:

-   true
-   false |
|orderActionType|String|Query|No|upgrade|The type of the configuration change. Valid values:

-   downgrade: downgrade
-   upgrade: upgrade |

## RequestBody

You also need to fill in the change item in RequestBody, an example is as follows.

```
{
  "nodeSpec": {
      "disk": 40
    }
} 
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|createdAt|String|2018-07-13T03:58:07.253Z|The time when the instance was created. |
|description|String|test|The name of the instance. |
|dictList|Array of DictList| |The configuration of the IK dictionary. |
|fileSize|Long|1000|The size of the Dictionary File. Unit: bytes. |
|name|String|test.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source type of the dictionary file. |
|type|String|MAIN|The type of the IK dictionary. Supported:

-   STOP: stopwords
-   MAIN: MAIN Dictionary
-   SYNONYMS: Synonym Dictionary
-   ALI\_WS: Alibaba Dictionary |
|domain|String|es-cn-abc.elasticsearch.aliyuncs.com|The private network access domain name of the instance. |
|esVersion|String|5.5.3\_with\_X-Pack|The version of the instance. |
|instanceId|String|es-cn-abc|The ID of the Elasticsearch instance. |
|kibanaConfiguration|Struct| |The configuration of Kibana nodes. |
|amount|Integer|1|The number of DRDS server nodes. |
|disk|Integer|20|The storage space size of the node. |
|diskType|String|cloud\_ssd|The storage type of the node. Ignore this parameter. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|kibanaDomain|String|es-cn-abc.kibana.elasticsearch.aliyuncs.com|The domain name used to access Kibana over a private network. |
|kibanaPort|Integer|5061|The port for Kibana access. |
|masterConfiguration|Struct| |The configuration of dedicated master nodes. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. Only cloud\_ssd is supported. |
|spec|String|elasticsearch.sn2ne.large|The node specification. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|The type of the network. Only Virtual Private Cloud \(VPC\) is supported. |
|vpcId|String|vpc-abc|The ID of the VPC. |
|vsArea|String|cn-hangzhou-a|The zone where the instance is deployed. |
|vswitchId|String|vsw-abc|The ID of the vSwitch in the specified VPC. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration of data nodes. |
|disk|Integer|40|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. Supported:

-   cloud\_ssd: standard SSD
-   cloud\_efficiency: ultra disk |
|spec|String|elasticsearch.sn2ne.xlarge|The specification of data nodes. |
|paymentType|String|postpaid|The billing method of the physical connection. Supported:

-   prepaid: Subscription
-   postpaid: pay-as-you-go |
|publicDomain|String|es-cn-abc.elasticsearch.aliyuncs.com|The Internet access domain name of the instance. |
|publicPort|Integer|8033|The public network access port of the instance. |
|status|String|active|The status of the cluster. Supported:

-   active: normal
-   activating: taking effect
-   inactive: Frozen
-   invalid: invalid |
|synonymsDicts|Array of SynonymsDicts| |The configuration of the synonym dictionary. |
|fileSize|Long|100|The size of the Dictionary File. Unit: bytes. |
|name|String|dicts.txt|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source of the synonym dictionary file. |
|type|String|MAIN|The type of the dictionary file. Supported:

-   STOP: stopwords
-   MAIN: MAIN Dictionary
-   SYNONYMS: Synonym Dictionary
-   ALI\_WS: Alibaba Dictionary |
|updatedAt|String|2018-07-18T10:10:04.484Z|The time when the instance was last updated. |

**Note:** In the following return example, only the parameters in the returned data list are guaranteed to be included in this article, and the parameters that are not mentioned are for reference only. It is not mandatory to obtain these parameters in the program.

## Examples

Sample requests

```
PUT /openapi/instances/es-cn-n6w1ptcb30009**** HTTP/1.1 
common request header 
{
  "nodeSpec": {
      "disk": 40
    }
}
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
        <disk>40</disk>
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
<RequestId>B5246080-9C30-4B6A-8F8A-8C705405****</RequestId>
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
            "disk": 40,
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
    "RequestId": "B5246080-9C30-4B6A-8F8A-8C705405****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

