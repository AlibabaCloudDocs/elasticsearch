# DescribeInstance

调用DescribeInstance，查询指定实例的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-s9dsk3k4k\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|advancedDedicateMaster|Boolean|true|是否包含专有主节点。 |
|advancedSetting|Struct| |高级配置。 |
|gcName|String|CMS|GC垃圾回收器名称。支持CMS、G1。 |
|aliwsDicts|Array of Dict| |阿里分词词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|aliws\_ext\_dict.txt|词典文件名称。 |
|sourceType|String|OSS|词典文件来源类型，取值：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）
-   ORIGIN：开源Elasticsearch
-   UPLOAD：上传的文件 |
|type|String|ALI\_WS|词典文件类型，取值：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|clientNodeConfiguration|Struct| |协调节点配置信息。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|40|节点存储空间大小，单位：GB。 |
|diskType|String|cloud\_efficiency|节点存储类型，只支持高效云盘（cloud\_efficiency）。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|createdAt|String|2018-07-13T03:58:07.253Z|实例创建时间。 |
|dedicateMaster|Boolean|false|是否包含专有主节点（旧版本）。 |
|description|String|es-cn-abc|实例名称。 |
|dictList|Array of DictList| |IK词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|词典文件来源类型，取值：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）
-   ORIGIN：开源Elasticsearch
-   UPLOAD：上传的文件 |
|type|String|MAIN|词典文件类型，取值：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|domain|String|es-cn-n6w1o1x0w001c\*\*\*\*.elasticsearch.aliyuncs.com|实例的内网地址。 |
|elasticDataNodeConfiguration|Struct| |弹性数据节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位：GB。 |
|diskEncryption|Boolean|true|是否为节点开启云盘加密。 |
|diskType|String|cloud\_ssd|节点存储类型。支持：

 -   cloud\_ssd：SSD云盘
-   cloud\_essd：ESSD云盘
-   cloud\_efficiency：高效云盘 |
|spec|String|elasticsearch.sn2ne.large|节点规格。 |
|enableKibanaPrivateNetwork|Boolean|false|是否开启Kibana私网访问。 |
|enableKibanaPublicNetwork|Boolean|true|是否开启Kibana公网访问。 |
|enablePublic|Boolean|true|是否开启实例的公网地址。 |
|esConfig|Map|\{"http.cors.allow-credentials":"false"\}|实例的YML文件配置信息。 |
|esIPBlacklist|List|\[ "0.0.0.0/0" \]|私网访问黑名单（已废弃）。 |
|esIPWhitelist|List|\[ "0.0.0.0/0" \]|私网访问白名单（已废弃）。 |
|esVersion|String|5.5.3\_with\_X-Pack|实例版本。 |
|extendConfigs|List|\[\{ "configType": "aliVersion","aliVersion": "ali1.3.0" \}\]|实例的扩展配置。 |
|haveClientNode|Boolean|true|是否包含协调节点。 |
|haveKibana|Boolean|true|是否包含Kibana节点。 |
|instanceId|String|es-cn-abc|实例ID。 |
|kibanaConfiguration|Struct| |Kibana节点的配置信息。 |
|amount|Integer|1|节点数量。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|kibanaDomain|String|es-cn-abc.kibana.elasticsearch.aliyuncs.com|Kibana地址。 |
|kibanaIPWhitelist|List|\[ "0.0.0.0/0" \]|Kibana公网地址访问白名单列表。 |
|kibanaPort|Integer|5601|Kibana的访问端口。 |
|kibanaPrivateIPWhitelist|List|\["192.168.xx.xx"\]|Kibana私网地址访问白名单列表。 |
|masterConfiguration|Struct| |Master节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|40|节点存储空间大小，单位：GB。 |
|diskType|String|cloud\_ssd|节点存储类型。只支持cloud\_ssd（SSD云盘）。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，只支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-abc|VPC网络ID。 |
|vsArea|String|cn-hangzhou-b|实例所在的可用区。 |
|vswitchId|String|vsw-abc|虚拟交换机ID。 |
|nodeAmount|Integer|2|实例的数据节点数量。 |
|nodeSpec|Struct| |数据节点配置信息。 |
|disk|Integer|0|节点存储空间大小，单位：GB。 |
|diskEncryption|Boolean|true|是否开启云盘加密：

 -   true：开启
-   false：不开启 |
|diskType|String|cloud\_ssd|节点磁盘类型。支持：cloud\_ssd（SSD云盘）、cloud\_efficiency（高效云盘）。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|paymentType|String|postpaid|实例的付费方式。支持：**prepaid**（包年包月）和**postpaid**（按量付费）。 |
|port|Integer|9200|实例的访问端口。 |
|privateNetworkIpWhiteList|List|0.0.0.0/0|实例的私网地址访问白名单列表。 |
|protocol|String|HTTP|访问协议。支持：**HTTP**和**HTTPS**。 |
|publicDomain|String|es-cn-abc.elasticsearch.aliyuncs.com|实例的公网地址。 |
|publicIpWhitelist|List|\[ "0.0.0.0/0" \]|实例的公网地址访问白名单列表。 |
|publicPort|Integer|9200|实例的公网访问端口。 |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|实例所属的资源组ID。 |
|status|String|active|实例的状态。支持：**active**（正常）、**activating**（生效中）、**inactive**（冻结）和**invalid**（失效）。 |
|synonymsDicts|Array of SynonymsDicts| |同义词词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|来源类型。 |
|type|String|STOP|词典类型。STOP：停用词、MAIN：主词典、SYNONYMS：同义词词典、ALI\_WS：阿里词典。 |
|tags|Array of Tag| |实例标签。 |
|tagKey|String|env|标签键。 |
|tagValue|String|dev|标签值。 |
|updatedAt|String|2018-07-13T03:58:07.253Z|实例最后更新时间。 |
|vpcInstanceId|String|vpc-bp1uag5jj38c\*\*\*\*|专有网络ID。 |
|warmNode|Boolean|true|是否开启冷数据节点。 |
|warmNodeConfiguration|Struct| |冷数据节点配置信息。 |
|amount|Integer|6|节点数量。 |
|disk|Integer|500|节点存储空间大小，单位：GB。 |
|diskEncryption|Boolean|true|是否开启云盘加密。 |
|diskType|String|cloud\_efficiency|节点存储空间类型。只支持cloud\_efficiency（高效云盘）。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|zoneCount|Integer|2|实例的可用区个数。 |
|zoneInfos|Array of ZoneInfo| |可用区信息。 |
|status|String|NORMAL|可用区状态。支持：**ISOLATION**（下线）、**NORMAL**（正常）。 |
|zoneId|String|cn-hangzhou-b|可用区ID。 |

**说明：** 以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
GET /openapi/instances/es-cn-s9dsk3k4k**** HTTP/1.1
公共请求头
```

正常返回示例

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

