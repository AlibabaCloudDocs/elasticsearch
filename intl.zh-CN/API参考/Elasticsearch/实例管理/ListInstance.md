# ListInstance

调用ListInstance，在列表中展示所有实例的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|page|Integer|否|1|实例列表的页码。

 起始值：**1**，默认值：**1**。 |
|size|Integer|否|10|分页查询时设置的每页条数。

 最大值：**100**，默认值：**10**。 |
|description|String|否|es-cn-abc|实例名称，支持模糊查询。例如搜索**abc**的所有实例，则可能返回**abc**、**abcde**、**xyabc**、**xabcy**的所有实例。 |
|instanceId|String|否|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|esVersion|String|否|5.3\_with\_X-Pack|实例版本。 |
|resourceGroupId|String|否|rg-aekzvowej3i\*\*\*\*|实例所在的资源组ID。 |
|tags|String|否|dev-env|实例标签。 |
|vpcId|String|否|vpc-bp16k1dvzxtmagcva\*\*\*\*|实例所在的专有网络ID。 |
|zoneId|String|否|cn-hangzhou-i|实例所在的可用区ID。 |
|paymentType|String|否|postpaid|实例的付费类型。可选值：postpaid（按量付费）、prepaid（包年包月）。 |
|instanceCategory|String|否|advanced|实例版本。可选值：x-pack（商业版）、advanced（增强版）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|10|实例总记录数。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Instance| |返回结果。 |
|advancedDedicateMaster|Boolean|false|是否包含专有主节点。 |
|clientNodeConfiguration|Struct| |协调节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_efficiency|节点的存储类型，只支持高效云盘（cloud\_efficiency）。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。 |
|createdAt|String|2018-07-13T03:58:07.253Z|实例创建时间。 |
|dedicateMaster|Boolean|false|是否包含专有主节点（旧版本）。 |
|description|String|es-cn-abc|实例名称。 |
|elasticDataNodeConfiguration|Struct| |弹性数据节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskEncryption|Boolean|true|是否为节点开启云盘加密。 |
|diskType|String|cloud\_ssd|节点存储类型。支持：cloud\_ssd（SSD云盘）、cloud\_essd（ESSD云盘）、cloud\_efficiency（高效云盘）。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。 |
|esVersion|String|5.5.3\_with\_X-Pack|实例版本。 |
|instanceId|String|es-cn-abc|实例ID。 |
|kibanaConfiguration|Struct| |Kibana节点配置。 |
|amount|Integer|1|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点存储类型。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|masterConfiguration|Struct| |Master节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点存储类型。只支持cloud\_ssd（SSD云盘）。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，只支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-abc|专有网络ID。 |
|vsArea|String|cn-hangzhou-e|实例所在的可用区。 |
|vswitchId|String|vsw-def|虚拟交换机ID。 |
|nodeAmount|Integer|2|实例的数据节点数量。 |
|nodeSpec|Struct| |数据节点配置信息。 |
|disk|Integer|50|节点的存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点的存储类型。支持：cloud\_ssd（SSD云盘）、cloud\_efficiency（高效云盘）。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|paymentType|String|postpaid|实例的付费方式。

 支持：**prepaid**（包年包月）和**postpaid**（按量付费）。 |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|资源组ID。 |
|status|String|active|实例的状态。

 支持：**active**（正常）、**activating**（生效中）、**inactive**（冻结）和**invalid**（失效）。 |
|tags|Array of Tag| |实例标签。 |
|tagKey|String|env|标签键。 |
|tagValue|String|dev|标签值。 |
|updatedAt|String|2018-07-18T10:10:04.484Z|实例最后更新的时间。 |

**说明：** 以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
GET /openapi/instances?description=abc&page=1&size=10
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>es-cn-n6w1ptcb30009****</instanceId>
    <version>5.5.3_with_X-Pack</version>
    <description>es-cn-n6w1ptcb30009****</description>
    <nodeAmount>3</nodeAmount>
    <paymentType>postpaid</paymentType>
    <status>active</status>
    <enablePublic>false</enablePublic>
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
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-hangzhou-i</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>elasticsearch</instanceType>
    <inited>true</inited>
    <esVersion>5.5.3_with_X-Pack</esVersion>
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
    <haveGrafana>false</haveGrafana>
    <haveCerebro>false</haveCerebro>
    <enableKibanaPublicNetwork>false</enableKibanaPublicNetwork>
    <enableKibanaPrivateNetwork>false</enableKibanaPrivateNetwork>
    <advancedSetting>
        <gcName>CMS</gcName>
    </advancedSetting>
</Result>
<RequestId>83EC620F-70A2-4497-A6A3-B1DF359D****</RequestId>
<Headers>
    <X-Total-Count>1</X-Total-Count>
</Headers>
```

`JSON` 格式

```
{
	"Result": [
		{
			"instanceId": "es-cn-n6w1ptcb30009****",
			"version": "5.5.3_with_X-Pack",
			"description": "es-cn-n6w1ptcb30009****",
			"nodeAmount": 3,
			"paymentType": "postpaid",
			"status": "active",
			"privateNetworkIpWhiteList": [],
			"enablePublic": false,
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
			"esVersion": "5.5.3_with_X-Pack",
			"esIPWhitelist": [],
			"esIPBlacklist": [],
			"kibanaIPWhitelist": [],
			"kibanaPrivateIPWhitelist": [],
			"publicIpWhitelist": [],
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
			"dictList": [],
			"synonymsDicts": [],
			"ikHotDicts": [],
			"aliwsDicts": [],
			"haveGrafana": false,
			"haveCerebro": false,
			"enableKibanaPublicNetwork": false,
			"enableKibanaPrivateNetwork": false,
			"advancedSetting": {
				"gcName": "CMS"
			}
		}
	],
	"RequestId": "83EC620F-70A2-4497-A6A3-B1DF359D****",
	"Headers": {
		"X-Total-Count": 1
	}
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

