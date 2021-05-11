# RestartInstance

调用RestartInstance，重启指定的阿里云Elasticsearch实例。

**说明：** 重启后，实例进入生效中（activing）状态。重启成功后，实例状态变为正常（active）。阿里云Elasticsearch支持单节点重启，节点重启分为普通重启和蓝绿重启。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=RestartInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/restart HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif1q8auz0003\*\*\*\*|实例ID。 |
|force|Boolean|Query|否|false|是否忽略集群状态，强制重启。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不值过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定重启参数信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|restartType

|String

|否

|instance

|重启类型。可选值：

 **instance**（默认）：实例重启。

 **nodeIp**：节点重启，需要指定节点的IP地址。

 **nodeEcsId**：节点重启，需要指定集群中ECS实例的ID。 |
|nodes

|List<String\>

|否

|\["127.0.0.1"\]

|选择节点重启时，指定待重启的节点的IP地址或ID。 |
|blueGreenDep

|Boolean

|否

|false

|节点重启时，是否启用蓝绿部署，默认值为**false**。 |
|batchCount

|Double

|否

|25.0

|实例强制重启时，设置的并发度。 |
|batchUnit

|String

|否

|percent

|**batchCount**的单位，默认为**percent**。 |

**说明：**

-   **restartType**不传或者传空字符串时，默认为**instance**。为**instance**时，默认忽略**blueGreenDep**参数，并且需要满足以下条件：
    -   **force**为**true**时，**batchCount**必须大于0，小于等于100，否则会报错RestartBatchValueError。
    -   **force**为**false**时，**batchCount**默认为0，设置为其他值时，会报错NormalRestartNotSupportBatch。
-   **restartType**为**nodeIp**时，默认忽略**batchCount**参数，并且需要满足以下条件：
    -   **nodes**不可为空，否则提醒参数错误。
    -   **blueGreenDep**为**true**时，在重启节点时，会启用蓝绿部署。为**false**时，不会启用蓝绿部署，即正常重启。

示例如下。

```

{
    "restartType":"nodeIp",
    "nodes":["172.16.xx.xx","172.16.xx.xx"],
    "blueGreenDep":true
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DC\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|createdAt|String|2020-07-06T10:18:48.662Z|实例创建时间。 |
|description|String|es-cn-abc|实例名称。 |
|dictList|Array of dictList| |IK词典配置。 |
|dictList| | | |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|来源类型，支持：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）
-   ORIGIN：开源Elasticsearch
-   UPLOAD：上传的文件 |
|type|String|MAIN|词典类型，取值：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|domain|String|es-cn-nif1q8auz0003\*\*\*\*.elasticsearch.aliyuncs.com|实例的内网访问地址。 |
|esVersion|String|6.7.0\_with\_X-Pack|实例版本。 |
|instanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|kibanaConfiguration|Struct| |Kibana节点配置。 |
|amount|Integer|1|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点存储类型。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|kibanaDomain|String|es-cn-nif1q8auz0003\*\*\*\*.kibana.elasticsearch.aliyuncs.com|Kibana公网访问地址。 |
|kibanaPort|Integer|5601|Kibana公网端口。 |
|masterConfiguration|Struct| |Master节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点存储类型。只支持cloud\_ssd（SSD云盘）。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，只支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-bp16k1dvzxtmagcva\*\*\*\*|专有网络ID。 |
|vsArea|String|cn-hangzhou-i|实例所在的可用区。 |
|vswitchId|String|vsw-bp1k4ec6s7sjdbudw\*\*\*\*|虚拟交换机ID。 |
|nodeAmount|Integer|2|实例的数据节点数量。 |
|nodeSpec|Struct| |数据节点配置信息。 |
|disk|Integer|50|节点的存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点的存储类型。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|paymentType|String|postpaid|实例的付费方式。

 支持：prepaid（包年包月）和postpaid（按量付费）。 |
|publicDomain|String|es-cn-n6w1o1x0w001c\*\*\*\*.public.elasticsearch.aliyuncs.com|公网访问地址。 |
|publicPort|Integer|9200|公网端口。 |
|status|String|active|实例的状态。

 支持：active（正常）、activating（生效中）、inactive（冻结）和invalid（失效）。 |
|synonymsDicts|Array of synonymsDicts| |同义词词典配置。 |
|synonymsDicts| | | |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|来源类型，支持：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）
-   ORIGIN：开源Elasticsearch
-   UPLOAD：上传的文件 |
|type|String|STOP|词典类型，取值：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|updatedAt|String|2018-07-18T10:10:04.484Z|实例最后更新的时间。 |

以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
POST /openapi/instances/es-cn-nif1q8auz0003****/actions/restart HTTP/1.1
公共请求头
{
    "restartType":"nodeIp",
    "nodes":["172.16.xx.xx","172.16.xx.xx"],
    "blueGreenDep":true
}
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"instanceId": "es-cn-nif1q8auz0003****",
		"version": "7.4.0_with_X-Pack",
		"description": "es-cn-nif1q8auz0003****",
		"nodeAmount": 3,
		"paymentType": "prepaid",
		"status": "active",
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
			"vpcId": "vpc-bp16k1dvzxtmagcva****",
			"vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
			"vsArea": "cn-hangzhou-i",
			"type": "vpc"
		},
		"createdAt": "2020-07-06T10:18:48.662Z",
		"updatedAt": "2020-07-06T10:18:48.662Z",
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
			}
		],
		"endTime": 1596729600000,
		"clusterTasks": [],
		"vpcInstanceId": "es-cn-nif1q8auz0003****-worker",
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
		"domain": "es-cn-nif1q8auz0003****.elasticsearch.aliyuncs.com",
		"port": 9200,
		"esVersion": "7.4.0_with_X-Pack",
		"esConfig": {
			"action.destructive_requires_name": "true",
			"xpack.watcher.enabled": "false",
			"action.auto_create_index": "+.*,-*"
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
		"publicIpWhitelist": [],
		"kibanaDomain": "es-cn-nif1q8auz0003****.kibana.elasticsearch.aliyuncs.com",
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
		}
	},
	"RequestId": "BB58A51D-CE72-49F9-AF08-F57F3C8A****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

