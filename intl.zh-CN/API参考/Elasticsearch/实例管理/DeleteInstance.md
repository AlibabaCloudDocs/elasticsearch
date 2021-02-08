# DeleteInstance

调用DeleteInstance，释放指定按量付费类型的阿里云Elasticsearch实例。释放后，实例所使用的物理资源都被回收，相关数据全部丢失且不可恢复；挂载实例节点的云盘和相应的快照都会被释放。

调用此接口前，需要注意：

实例释放后数据无法恢复，建议您在释放前先备份数据。具体操作，请参见[快照备份与恢复命令](~~65675~~)。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DeleteInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/instances/[InstanceId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-t57p81n7ai89v\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|deleteType|String|Query|否|protective|释放类型。可选值：

 -   immediate：立即删除。删除后，系统会彻底清除所有数据，且实例不再显示在实例列表中。
-   protective：实例会被冻结24小时后，再彻底清除数据，期间实例仍在实例列表中显示，您可以选择[恢复实例](~~202195~~)或[立即释放](~~202195~~)。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|94B03BBA-A132-42C3-8367-0A0C1C45\*\*\*\*|请求ID。 |

返回数据中还包括Result参数。详细信息，请参见[ListInstance](~~142230~~)。

## 示例

请求示例

```
DELETE /openapi/instances/es-cn-t57p81n7ai89v****?deleteType=protective HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

