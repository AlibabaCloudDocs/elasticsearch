# UpdateInstance

调用UpdateInstance，变更集群配置（升配或降配）。

调用该接口时，请注意：

-   当实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法变更配置。
-   每次只能变更一种类型的节点（数据节点、专有主节点、冷数据节点、协调节点、Kibana节点、弹性节点）的配置。更多注意事项，请参见[升配集群](~~96650~~)和[降配集群](~~198887~~)。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/instances/[InstanceId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1ptcb30009\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|ignoreStatus|Boolean|Query|否|true|变更集群配置时，是否忽略集群状态：

 -   true：是
-   false：否 |
|orderActionType|String|Query|否|upgrade|配置变更类型，可选值：

 -   downgrade：降配
-   upgrade：升配 |

## RequestBody

RequestBody中还需要填写变更项，示例如下。

```

{
  "nodeSpec": {
      "disk": 40
    }
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|createdAt|String|2018-07-13T03:58:07.253Z|实例创建时间。 |
|description|String|test|实例名称。 |
|dictList|Array of DictList| |IK词典配置。 |
|fileSize|Long|1000|词典文件大小，单位：字节。 |
|name|String|test.dic|词典文件名称。 |
|sourceType|String|ORIGIN|词典文件来源类型。 |
|type|String|MAIN|IK词典类型。支持：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|domain|String|es-cn-abc.elasticsearch.aliyuncs.com|实例的私网访问域名。 |
|esVersion|String|5.5.3\_with\_X-Pack|实例版本。 |
|instanceId|String|es-cn-abc|实例ID。 |
|kibanaConfiguration|Struct| |Kibana节点配置。 |
|amount|Integer|1|节点数量。 |
|disk|Integer|20|节点存储空间大小。 |
|diskType|String|cloud\_ssd|节点存储类型（可忽略该参数）。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|kibanaDomain|String|es-cn-abc.kibana.elasticsearch.aliyuncs.com|Kibana私网访问域名。 |
|kibanaPort|Integer|5061|Kibana访问端口。 |
|masterConfiguration|Struct| |Master节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点的存储空间大小，单位：GB。 |
|diskType|String|cloud\_ssd|节点的存储类型。只支持cloud\_ssd（SSD云盘）。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，只支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-abc|专有网络ID。 |
|vsArea|String|cn-hangzhou-a|实例所在的可用区。 |
|vswitchId|String|vsw-abc|虚拟交换机ID。 |
|nodeAmount|Integer|2|数据节点的数量。 |
|nodeSpec|Struct| |数据节点配置信息。 |
|disk|Integer|40|节点存储空间大小，单位：GB。 |
|diskType|String|cloud\_ssd|节点存储类型。支持：

 -   cloud\_ssd：SSD云盘
-   cloud\_efficiency：高效云盘 |
|spec|String|elasticsearch.sn2ne.xlarge|节点规格。 |
|paymentType|String|postpaid|实例的付费方式。支持：

 -   prepaid：包年包月
-   postpaid：按量付费 |
|publicDomain|String|es-cn-abc.elasticsearch.aliyuncs.com|实例的公网访问域名。 |
|publicPort|Integer|8033|实例的公网访问端口。 |
|status|String|active|实例的状态。支持：

 -   active：正常
-   activating：生效中
-   inactive：冻结
-   invalid：失效 |
|synonymsDicts|Array of SynonymsDicts| |同义词词典配置。 |
|fileSize|Long|100|词典文件大小，单位：字节。 |
|name|String|dicts.txt|词典文件名称。 |
|sourceType|String|ORIGIN|来源类型。 |
|type|String|MAIN|词典文件类型。支持：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|updatedAt|String|2018-07-18T10:10:04.484Z|实例最后更新时间。 |

**说明：** 以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
PUT /openapi/instances/es-cn-n6w1ptcb30009**** HTTP/1.1
公共请求头
{
  "nodeSpec": {
      "disk": 40
    }
}
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

