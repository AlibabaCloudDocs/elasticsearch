# MoveResourceGroup

Call the MoveResourceGroup to migrate the instance to the specified resource group.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=MoveResourceGroup&type=ROA&version=2017-06-13)

## Request headers

This operation uses common request headers, but does not use special request headers. For more information, see Common parameters.

## Request syntax

```
PATCH|POST|PUT /openapi/instances/[InstanceId]/resourcegroup HTTP/1.1
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

Enter the following parameters in RequestBody to specify the resource group to which the instance will be migrated.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|resourceGroupId

|String

|Yes

|rg-acfm2h5vbzd\*\*\*\*

|The ID of the resource group. Available at [Resource Groups](https://resourcemanager.console.aliyun.com/resource-groups)Get on page. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|createdAt|String|2020-07-06T10:18:48.662Z|The time when the instance was created. |
|description|String|es-cn-abc|The name of the instance. |
|dictList|Array of dictList| |The configuration of the IK dictionary. |
|fileSize|Long|2782602|The size of the Dictionary File. Unit: bytes. |
|name|String|SYSTEM\_MAIN.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source type. Valid values:

-   OSS:OSS open storage \(make sure the OSS bucket is publicly readable.\)
-   ORIGIN: Retains dictionaries that have been uploaded. |
|type|String|MAIN|The type of the dictionary. Valid values:

-   STOP: Stopwords
-   MAIN: MAIN dictionary
-   SYNONYMS: Synonym dictionary
-   ALI\_WS: Alibaba dictionary |
|domain|String|es-cn-nif1q8auz0003\*\*\*\*.elasticsearch.aliyuncs.com|The internal network endpoint of the instance. |
|esVersion|String|6.7.0\_with\_X-Pack|The version of the instance. |
|instanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|kibanaConfiguration|Struct| |The configuration of Kibana nodes. |
|amount|Integer|1|The number of DRDS server nodes. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|kibanaDomain|String|es-cn-nif1q8auz0003\*\*\*\*.kibana.elasticsearch.aliyuncs.com|The public network endpoint of Kibana. |
|kibanaPort|Integer|5601|The public port of Kibana. |
|masterConfiguration|Struct| |The configuration of dedicated master nodes. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. |
|spec|String|elasticsearch.sn2ne.large|The specification of the node. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|The type of the network. Only Virtual Private Cloud is supported. |
|vpcId|String|vpc-bp16k1dvzxtmagcva\*\*\*\*|The ID of the VPC. |
|vsArea|String|cn-hangzhou-i|The zone where the instance is deployed. |
|vswitchId|String|vsw-bp1k4ec6s7sjdbudw\*\*\*\*|The ID of the vSwitch. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration of data nodes. |
|disk|Integer|50|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|paymentType|String|postpaid|The billing method of the physical connection. Valid values:

-   prepaid: subscription
-   postpaid: pay-as-you-go |
|publicDomain|String|es-cn-n6w1o1x0w001c\*\*\*\*.public.elasticsearch.aliyuncs.com|Public network access address. |
|publicPort|Integer|9200|The public network port. |
|status|String|active|The status of the cluster. Valid values:

-   active: normal
-   activating: taking effect
-   inactive: frozen
-   invalid: invalid |
|synonymsDicts|Array of synonymsDicts| |The configuration of the synonym dictionary. |
|fileSize|Long|2782602|The size of the Dictionary File. Unit: bytes. |
|name|String|SYSTEM\_MAIN.dic|The name of the dictionary file. |
|sourceType|String|ORIGIN|The source type. Valid values:

-   OSS:OSS open storage \(make sure the OSS bucket is publicly readable.\)
-   ORIGIN: Retains dictionaries that have been uploaded. |
|type|String|STOP|The type of the dictionary. Valid values:

-   STOP: Stopwords
-   MAIN: MAIN dictionary
-   SYNONYMS: Synonym dictionary
-   ALI\_WS: Alibaba dictionary |
|updatedAt|String|2018-07-18T10:10:04.484Z|The time when the instance was last updated. |

## Examples

Sample requests

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/resourcegroup HTTP/1.1 
common request header
{
   "resourceGroupId": "rg-acfm2h5vbzd****"
}   
```

Sample success responses

`XML` format

```
<Result>
    <dryRun>false</dryRun>
    <id>24210</id>
    <instanceId>es-cn-npk2151ww000a****</instanceId>
    <version>6.7.0_with_X-Pack</version>
    <description/>
    <ownerId>168520994880****</ownerId>
    <nodeAmount>3</nodeAmount>
    <paymentType>prepaid</paymentType>
    <status>inactive</status>
    <privateNetworkIpWhiteList>0.0.0.0/0</privateNetworkIpWhiteList>
    <enablePublic>false</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.sn2ne.2xlarge</spec>
        <disk>20</disk>
        <diskType>cloud_ssd</diskType>
        <diskEncryption>false</diskEncryption>
        <empty>false</empty>
    </nodeSpec>
    <networkConfig>
        <vpcId>vpc-bp12nu14urf0upaf4****</vpcId>
        <vswitchId>vsw-bp1xn8mossizozn6q****</vswitchId>
        <vsArea>cn-hangzhou-g</vsArea>
        <type>vpc</type>
    </networkConfig>
    <lastPayTime>2021-03-03T16:00:58.000+0000</lastPayTime>
    <createdAt>2021-02-03T12:22:14.643Z</createdAt>
    <updatedAt>2021-03-04T03:46:02.358Z</updatedAt>
    <commodityCode>elasticsearchpre</commodityCode>
    <extendConfigs>
        <configType>usageScenario</configType>
        <value>general</value>
    </extendConfigs>
    <extendConfigs>
        <configType>maintainTime</configType>
        <maintainStartTime>02:00Z</maintainStartTime>
        <maintainEndTime>06:00Z</maintainEndTime>
    </extendConfigs>
    <extendConfigs>
        <configType>aliVersion</configType>
        <aliVersion>ali1.3.0</aliVersion>
    </extendConfigs>
    <endTime>1614787200000</endTime>
    <force>false</force>
    <ignoreStatus>false</ignoreStatus>
    <vpcInstanceId>es-cn-npk2151ww000a****-worker</vpcInstanceId>
    <resourceGroupId>rg-aek2wq2jlqd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-hangzhou-g</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>elasticsearch</instanceType>
    <inited>true</inited>
    <tags>
        <tagKey>acs:rm:rgId</tagKey>
        <tagValue>rg-acfm2h5vbzd****</tagValue>
    </tags>
    <domain>es-cn-npk2151ww000a****.elasticsearch.aliyuncs.com</domain>
    <port>9200</port>
    <esVersion>6.7.0_with_X-Pack</esVersion>
    <esConfig>
        <action.destructive_requires_name>true</action.destructive_requires_name>
        <xpack.security.audit.outputs>index</xpack.security.audit.outputs>
        <xpack.watcher.enabled>false</xpack.watcher.enabled>
        <xpack.security.audit.enabled>false</xpack.security.audit.enabled>
        <action.auto_create_index>+.*,-*</action.auto_create_index>
    </esConfig>
    <esIPWhitelist>0.0.0.0/0</esIPWhitelist>
    <kibanaProtocol>HTTPS</kibanaProtocol>
    <kibanaIPWhitelist>::1</kibanaIPWhitelist>
    <kibanaIPWhitelist>127.0.0.1</kibanaIPWhitelist>
    <kibanaDomain>es-cn-npk2151ww000a****.kibana.elasticsearch.aliyuncs.com</kibanaDomain>
    <kibanaPort>5601</kibanaPort>
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
    <elasticDataNodeConfiguration/>
    <haveElasticDataNode>false</haveElasticDataNode>
    <dictList>
        <name>SYSTEM_MAIN.dic</name>
        <fileSize>2782602</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>MAIN</type>
    </dictList>
    <dictList>
        <name>SYSTEM_STOPWORD.dic</name>
        <fileSize>132</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>STOP</type>
    </dictList>
    <aliwsDicts>
        <name>aliws_ext_dict.txt</name>
        <fileSize>17</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>ALI_WS</type>
    </aliwsDicts>
    <haveGrafana>false</haveGrafana>
    <haveCerebro>false</haveCerebro>
    <enableKibanaPublicNetwork>true</enableKibanaPublicNetwork>
    <enableKibanaPrivateNetwork>false</enableKibanaPrivateNetwork>
    <advancedSetting>
        <gcName>CMS</gcName>
    </advancedSetting>
    <enableMetrics>true</enableMetrics>
    <readWritePolicy>
        <writeHa>false</writeHa>
    </readWritePolicy>
</Result>
<RequestId>D63D226F-2B80-4BF9-8F70-DBEDCC8DC2AF</RequestId>
```

`JSON` format

```
{
  "Result": {
    "dryRun": false,
    "id": "24210",
    "instanceId": "es-cn-npk2151ww000a****",
    "version": "6.7.0_with_X-Pack",
    "description": "",
    "ownerId": "168520994880****",
    "nodeAmount": 3,
    "paymentType": "prepaid",
    "status": "inactive",
    "privateNetworkIpWhiteList": [
      "0.0.0.0/0"
    ],
    "enablePublic": false,
    "nodeSpec": {
      "spec": "elasticsearch.sn2ne.2xlarge",
      "disk": 20,
      "diskType": "cloud_ssd",
      "diskEncryption": false,
      "empty": false
    },
    "networkConfig": {
      "vpcId": "vpc-bp12nu14urf0upaf4****",
      "vswitchId": "vsw-bp1xn8mossizozn6q****",
      "vsArea": "cn-hangzhou-g",
      "type": "vpc"
    },
    "lastPayTime": "2021-03-03T16:00:58.000+0000",
    "createdAt": "2021-02-03T12:22:14.643Z",
    "updatedAt": "2021-03-04T03:46:02.358Z",
    "commodityCode": "elasticsearchpre",
    "extendConfigs": [
      {
        "configType": "usageScenario",
        "value": "general"
      },
      {
        "configType": "maintainTime",
        "maintainStartTime": "02:00Z",
        "maintainEndTime": "06:00Z"
      },
      {
        "configType": "aliVersion",
        "aliVersion": "ali1.3.0"
      }
    ],
    "endTime": 1614787200000,
    "force": false,
    "ignoreStatus": false,
    "clusterTasks": [],
    "vpcInstanceId": "es-cn-npk2151ww000a****-worker",
    "resourceGroupId": "rg-aek2wq2jlqd****",
    "zoneCount": 1,
    "protocol": "HTTP",
    "zoneInfos": [
      {
        "zoneId": "cn-hangzhou-g",
        "status": "NORMAL"
      }
    ],
    "instanceType": "elasticsearch",
    "inited": true,
    "tags": [
      {
        "tagKey": "acs:rm:rgId",
        "tagValue": "rg-acfm2h5vbzd****"
      }
    ],
    "customerLabels": [],
    "domain": "es-cn-npk2151ww000a****.elasticsearch.aliyuncs.com",
    "port": 9200,
    "esVersion": "6.7.0_with_X-Pack",
    "esConfig": {
      "action.destructive_requires_name": "true",
      "xpack.security.audit.outputs": "index",
      "xpack.watcher.enabled": "false",
      "xpack.security.audit.enabled": "false",
      "action.auto_create_index": "+.*,-*"
    },
    "esIPWhitelist": [
      "0.0.0.0/0"
    ],
    "esIPBlacklist": [],
    "kibanaProtocol": "HTTPS",
    "kibanaIPWhitelist": [
      "::1",
      "127.0.0.1"
    ],
    "kibanaPrivateIPWhitelist": [],
    "publicIpWhitelist": [],
    "kibanaDomain": "es-cn-npk2151ww000a****.kibana.elasticsearch.aliyuncs.com",
    "kibanaPort": 5601,
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
    "elasticDataNodeConfiguration": {},
    "haveElasticDataNode": false,
    "dictList": [
      {
        "name": "SYSTEM_MAIN.dic",
        "fileSize": 2782602,
        "sourceType": "ORIGIN",
        "type": "MAIN"
      },
      {
        "name": "SYSTEM_STOPWORD.dic",
        "fileSize": 132,
        "sourceType": "ORIGIN",
        "type": "STOP"
      }
    ],
    "synonymsDicts": [],
    "ikHotDicts": [],
    "aliwsDicts": [
      {
        "name": "aliws_ext_dict.txt",
        "fileSize": 17,
        "sourceType": "ORIGIN",
        "type": "ALI_WS"
      }
    ],
    "haveGrafana": false,
    "haveCerebro": false,
    "enableKibanaPublicNetwork": true,
    "enableKibanaPrivateNetwork": false,
    "advancedSetting": {
      "gcName": "CMS"
    },
    "enableMetrics": true,
    "readWritePolicy": {
      "writeHa": false
    }
  },
  "RequestId": "D63D226F-2B80-4BF9-8F70-DBEDCC8DC2AF"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

