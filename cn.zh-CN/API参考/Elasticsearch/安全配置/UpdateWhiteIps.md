# UpdateWhiteIps

调用UpdateWhiteIps，更新Elasticsearch实例的VPC私网访问白名单。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateWhiteIps&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/white-ips HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-npk2154oi000b\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|esIPWhitelist|List<String\>|Body|否|\["10.61.xx.xx", "106.11.xx.xx”\]|白名单列表。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|8D58B014-BBD7-4D80-B219-00B9D5C6860C|请求ID。 |
|Result|Struct| |返回结果。 |
|esIPWhitelist|List|\["106.11.xx.xx", "10.61.xx.xx"\]|更新后的白名单列表。 |

**说明：** 返回数据中还包含实例的其他信息，具体请参见[ListLogstash](~~160534~~)。

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-npk2154oi000b****/white-ips?esIPWhitelist="[\"10.61.xx.xx\", \"106.11.xx.xx\"]" HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <instanceId>es-cn-npk2154oi000b****</instanceId>
    <version>6.7.0_with_X-Pack</version>
    <description>es-cn-npk2154oi000b****</description>
    <nodeAmount>3</nodeAmount>
    <paymentType>prepaid</paymentType>
    <status>active</status>
    <privateNetworkIpWhiteList>106.11.xx.xx</privateNetworkIpWhiteList>
    <privateNetworkIpWhiteList>10.61.xx.xx</privateNetworkIpWhiteList>
    <enablePublic>false</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.sn2ne.large</spec>
        <disk>23</disk>
        <diskType>cloud_ssd</diskType>
        <diskEncryption>false</diskEncryption>
    </nodeSpec>
    <networkConfig>
        <vpcId>vpc-bp12t9s2wbajnm6wr****</vpcId>
        <vswitchId>vsw-bp1ndheong7s2dio7****</vswitchId>
        <vsArea>cn-hangzhou-g</vsArea>
        <type>vpc</type>
    </networkConfig>
    <createdAt>2021-02-03T13:22:02.309Z</createdAt>
    <updatedAt>2021-02-14T07:33:22.686Z</updatedAt>
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
    <vpcInstanceId>es-cn-npk2154oi000b****-worker</vpcInstanceId>
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
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
    <domain>es-cn-npk2154oi000b****.elasticsearch.aliyuncs.com</domain>
    <port>9200</port>
    <esVersion>6.7.0_with_X-Pack</esVersion>
    <esConfig>
        <action.destructive_requires_name>true</action.destructive_requires_name>
        <xpack.security.audit.outputs>index</xpack.security.audit.outputs>
        <xpack.watcher.enabled>false</xpack.watcher.enabled>
        <xpack.security.audit.enabled>false</xpack.security.audit.enabled>
        <action.auto_create_index>+.*,-*</action.auto_create_index>
    </esConfig>
    <esIPWhitelist>106.11.xx.xx</esIPWhitelist>
    <esIPWhitelist>10.61.xx.xx</esIPWhitelist>
    <kibanaProtocol>HTTPS</kibanaProtocol>
    <kibanaIPWhitelist>::1</kibanaIPWhitelist>
    <kibanaIPWhitelist>127.0.0.1</kibanaIPWhitelist>
    <kibanaDomain>es-cn-npk2154oi000b****.kibana.elasticsearch.aliyuncs.com</kibanaDomain>
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
        <name>dict_0.dic</name>
        <fileSize>220</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>MAIN</type>
    </dictList>
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
    <synonymsDicts>
        <name>aliws_ext_dict.txt</name>
        <fileSize>17</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>SYNONYMS</type>
    </synonymsDicts>
    <ikHotDicts>
        <name>dict_0.dic</name>
        <fileSize>220</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>MAIN</type>
    </ikHotDicts>
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
<RequestId>8D58B014-BBD7-4D80-B219-00B9D5C6860C</RequestId>
```

`JSON`格式

```
{
  "Result": {
    "instanceId": "es-cn-npk2154oi000b****",
    "version": "6.7.0_with_X-Pack",
    "description": "es-cn-npk2154oi000b****",
    "nodeAmount": 3,
    "paymentType": "prepaid",
    "status": "active",
    "privateNetworkIpWhiteList": [
      "106.11.xx.xx",
      "10.61.xx.xx"
    ],
    "enablePublic": false,
    "nodeSpec": {
      "spec": "elasticsearch.sn2ne.large",
      "disk": 23,
      "diskType": "cloud_ssd",
      "diskEncryption": false
    },
    "networkConfig": {
      "vpcId": "vpc-bp12t9s2wbajnm6wr****",
      "vswitchId": "vsw-bp1ndheong7s2dio7****",
      "vsArea": "cn-hangzhou-g",
      "type": "vpc"
    },
    "createdAt": "2021-02-03T13:22:02.309Z",
    "updatedAt": "2021-02-14T07:33:22.686Z",
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
    "clusterTasks": [],
    "vpcInstanceId": "es-cn-npk2154oi000b****-worker",
    "resourceGroupId": "rg-acfm2h5vbzd****",
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
    "domain": "es-cn-npk2154oi000b****.elasticsearch.aliyuncs.com",
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
      "106.11.xx.xx",
      "10.61.xx.xx"
    ],
    "esIPBlacklist": [],
    "kibanaProtocol": "HTTPS",
    "kibanaIPWhitelist": [
      "::1",
      "127.0.0.1"
    ],
    "kibanaPrivateIPWhitelist": [],
    "publicIpWhitelist": [],
    "kibanaDomain": "es-cn-npk2154oi000b****.kibana.elasticsearch.aliyuncs.com",
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
        "name": "dict_0.dic",
        "fileSize": 220,
        "sourceType": "ORIGIN",
        "type": "MAIN"
      },
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
    "synonymsDicts": [
      {
        "name": "aliws_ext_dict.txt",
        "fileSize": 17,
        "sourceType": "ORIGIN",
        "type": "SYNONYMS"
      }
    ],
    "ikHotDicts": [
      {
        "name": "dict_0.dic",
        "fileSize": 220,
        "sourceType": "ORIGIN",
        "type": "MAIN"
      }
    ],
    "aliwsDicts": [],
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
  "RequestId": "8D58B014-BBD7-4D80-B219-00B9D5C6860C"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

