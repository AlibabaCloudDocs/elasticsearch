# DeleteInstance

Call the DeleteInstance to release an Alibaba Cloud Elasticsearch instance of the specified pay-as-you-go type. After the instance is released, the physical resources of the instance is reclaimed. The data of the instance is deleted and cannot be recovered. The disks mounted to the instance nodes and the snapshots are released.

Before calling this interface, note the following:

After an instance is released, its data cannot be recovered. We recommend that you back up your data before releasing an instance. For more information, see [Commands to create snapshots and restore data](~~65675~~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DeleteInstance&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
DELETE /openapi/instances/[InstanceId] HTTP/1.1 
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-t57p81n7ai89v\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|deleteType|String|Query|No|protective|The release type. Valid values:

-   immediate: the object is deleted immediately. After the instance is deleted, all data is completely cleared and the instance is no longer displayed in the instance list.
-   protective: The instance will be frozen for 24 hours before the data is completely cleared. During this time, the instance is still displayed in the instance List. You can choose [Restore an instance](~~202195~~) or [Release now](~~202195~~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|94B03BBA-A132-42C3-8367-0A0C1C45\*\*\*\*|The ID of the request. |

The returned data also includes the Result parameter. For more information, see [ListInstance](~~142230~~).

## Examples

Sample requests

```
DELETE /openapi/instances/es-cn-t57p81n7ai89v****? deleteType=protective HTTP/1.1 
common request headers
```

Sample success responses

`XML`format

```
<Result>
    <instanceId>es-cn-z2q1wk6z00007****</instanceId>
    <version>6.7.0_with_X-Pack</version>
    <description>es-cn-z2q1wk6z00007****</description>
    <nodeAmount>3</nodeAmount>
    <paymentType>postpaid</paymentType>
    <status>inactive</status>
    <privateNetworkIpWhiteList>0.0.0.0/0</privateNetworkIpWhiteList>
    <enablePublic>false</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.n4.small</spec>
        <disk>20</disk>
        <diskType>cloud_ssd</diskType>
        <diskEncryption>false</diskEncryption>
    </nodeSpec>
    <networkConfig>
        <vpcId>vpc-uf6ov5av2bmjgzrfg****</vpcId>
        <vswitchId>vsw-uf6r9op1zf38byetr****</vswitchId>
        <vsArea>cn-shanghai-g</vsArea>
        <type>vpc</type>
    </networkConfig>
    <createdAt>2020-11-06T11:47:27.776Z</createdAt>
    <updatedAt>2021-02-01T02:04:19.844Z</updatedAt>
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
    <extendConfigs>
        <configType>aliVersion</configType>
        <aliVersion>ali1.3.0</aliVersion>
    </extendConfigs>
    <extendConfigs>
        <configType>deleteProtection</configType>
        <deleteTime>1612231459805</deleteTime>
    </extendConfigs>
    <endTime>4760352000000</endTime>
    <vpcInstanceId>es-cn-z2q1wk6z00007****-worker</vpcInstanceId>
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-shanghai-g</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>elasticsearch</instanceType>
    <inited>true</inited>
    <tags>
        <tagKey>acs:rm:rgId</tagKey>
        <tagValue>rg-acfm2h5vbzd****</tagValue>
    </tags>
    <domain>es-cn-z2q1wk6z00007****.elasticsearch.aliyuncs.com</domain>
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
    <kibanaIPWhitelist>0.0.0.0/0</kibanaIPWhitelist>
    <kibanaDomain>es-cn-z2q1wk6z00007****.kibana.elasticsearch.aliyuncs.com</kibanaDomain>
    <kibanaPort>5601</kibanaPort>
    <haveKibana>true</haveKibana>
    <instanceCategory>x-pack</instanceCategory>
    <dedicateMaster>false</dedicateMaster>
    <advancedDedicateMaster>true</advancedDedicateMaster>
    <masterConfiguration>
        <spec>elasticsearch.sn2ne.large</spec>
        <amount>3</amount>
        <diskType>cloud_ssd</diskType>
        <disk>20</disk>
    </masterConfiguration>
    <haveClientNode>false</haveClientNode>
    <warmNode>false</warmNode>
    <warmNodeConfiguration/>
    <clientNodeConfiguration/>
    <kibanaConfiguration>
        <spec>elasticsearch.n4.small</spec>
        <amount>1</amount>
        <disk>0</disk>
    </kibanaConfiguration>
    <elasticDataNodeConfiguration>
        <spec>elasticsearch.sn1ne.large</spec>
        <amount>3</amount>
        <diskType>cloud_efficiency</diskType>
        <disk>20480</disk>
        <diskEncryption>false</diskEncryption>
    </elasticDataNodeConfiguration>
    <haveElasticDataNode>true</haveElasticDataNode>
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
<RequestId>898EF65A-121E-4728-93E3-3412DAA8****</RequestId>
```

`JSON`Syntax

```
{
  "Result": {
    "instanceId": "es-cn-z2q1wk6z00007****",
    "version": "6.7.0_with_X-Pack",
    "description": "es-cn-z2q1wk6z00007****",
    "nodeAmount": 3,
    "paymentType": "postpaid",
    "status": "inactive",
    "privateNetworkIpWhiteList": [
      "0.0.0.0/0"
    ],
    "enablePublic": false,
    "nodeSpec": {
      "spec": "elasticsearch.n4.small",
      "disk": 20,
      "diskType": "cloud_ssd",
      "diskEncryption": false
    },
    "networkConfig": {
      "vpcId": "vpc-uf6ov5av2bmjgzrfg****",
      "vswitchId": "vsw-uf6r9op1zf38byetr****",
      "vsArea": "cn-shanghai-g",
      "type": "vpc"
    },
    "createdAt": "2020-11-06T11:47:27.776Z",
    "updatedAt": "2021-02-01T02:04:19.844Z",
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
      },
      {
        "configType": "aliVersion",
        "aliVersion": "ali1.3.0"
      },
      {
        "configType": "deleteProtection",
        "deleteTime": 1612231459805
      }
    ],
    "endTime": 4760352000000,
    "clusterTasks": [],
    "vpcInstanceId": "es-cn-z2q1wk6z00007****-worker",
    "resourceGroupId": "rg-acfm2h5vbzd****",
    "zoneCount": 1,
    "protocol": "HTTP",
    "zoneInfos": [
      {
        "zoneId": "cn-shanghai-g",
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
    "domain": "es-cn-z2q1wk6z00007****.elasticsearch.aliyuncs.com",
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
      "0.0.0.0/0"
    ],
    "kibanaPrivateIPWhitelist": [],
    "publicIpWhitelist": [],
    "kibanaDomain": "es-cn-z2q1wk6z00007****.kibana.elasticsearch.aliyuncs.com",
    "kibanaPort": 5601,
    "haveKibana": true,
    "instanceCategory": "x-pack",
    "dedicateMaster": false,
    "advancedDedicateMaster": true,
    "masterConfiguration": {
      "spec": "elasticsearch.sn2ne.large",
      "amount": 3,
      "diskType": "cloud_ssd",
      "disk": 20
    },
    "haveClientNode": false,
    "warmNode": false,
    "warmNodeConfiguration": {},
    "clientNodeConfiguration": {},
    "kibanaConfiguration": {
      "spec": "elasticsearch.n4.small",
      "amount": 1,
      "disk": 0
    },
    "elasticDataNodeConfiguration": {
      "spec": "elasticsearch.sn1ne.large",
      "amount": 3,
      "diskType": "cloud_efficiency",
      "disk": 20480,
      "diskEncryption": false
    },
    "haveElasticDataNode": true,
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
  "RequestId": "898EF65A-121E-4728-93E3-3412DAA8****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

