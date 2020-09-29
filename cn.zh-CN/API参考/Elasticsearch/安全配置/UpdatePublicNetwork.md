# UpdatePublicNetwork

调用UpdatePublicNetwork，开启或关闭指定Elasticsearch实例的公网地址。

调用该接口时，请注意：

当实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法更新信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdatePublicNetwork&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/public-network HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-nif1q9o8r0008\*\*\*\*|实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

您还可以在RequestBody中填入**enablePublic**参数（可选，默认为false）。Boolean类型，为true时表示开启公网地址访问，为false时表示关闭公网地址访问，示例如下。

```

{
  "enablePublic": true
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|2A88ECA1-D827-4581-AD39-05149586\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|enablePublic|Boolean|false|公网地址开关状态。 |

**说明：** 下文返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，参数说明请参见[ListInstance](~~142230~~)。程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-nif1q9o8r0008****/public-network HTTP/1.1
公共请求头
{
  "enablePublic": true
}
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>es-cn-nif1q9o8r0008****</instanceId>
    <version>6.7.0_with_X-Pack</version>
    <description>es-cn-nif1q9o8r0008****</description>
    <nodeAmount>4</nodeAmount>
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
    <createdAt>2020-07-07T04:05:16.791Z</createdAt>
    <updatedAt>2020-07-07T10:15:14.498Z</updatedAt>
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
        <aliVersion>ali1.2.0</aliVersion>
    </extendConfigs>
    <endTime>4749811200000</endTime>
    <clusterTasks>
        <type>MigrateData</type>
        <progress>100</progress>
        <detail/>
        <status>FINISHED</status>
        <canCancelable>false</canCancelable>
        <interruptible>false</interruptible>
        <subTasks>
            <type>FindShrinkNodeAction</type>
            <progress>100</progress>
            <detail/>
            <status>FINISHED</status>
            <canCancelable>false</canCancelable>
            <interruptible>false</interruptible>
        </subTasks>
        <subTasks>
            <type>MigrateDataAction</type>
            <progress>100</progress>
            <detail>
                <doneMigrateNodeIps>172.16.**.**</doneMigrateNodeIps>
                <allMigrateNodeIps>172.16.**.**</allMigrateNodeIps>
            </detail>
            <status>FINISHED</status>
            <canCancelable>false</canCancelable>
            <interruptible>false</interruptible>
        </subTasks>
    </clusterTasks>
    <vpcInstanceId>es-cn-nif1q9o8r0008****-worker</vpcInstanceId>
    <resourceGroupId>rg-acfm2h5vbzdokea</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-hangzhou-i</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>elasticsearch</instanceType>
    <inited>true</inited>
    <domain>es-cn-nif1q9o8r0008****.elasticsearch.aliyuncs.com</domain>
    <port>9200</port>
    <esVersion>6.7.0_with_X-Pack</esVersion>
    <esConfig>
        <thread_pool.bulk.queue_size>500</thread_pool.bulk.queue_size>
    </esConfig>
    <esIPWhitelist>0.0.0.0/0</esIPWhitelist>
    <kibanaIPWhitelist>0.0.0.0/0</kibanaIPWhitelist>
    <kibanaIPWhitelist>::/0</kibanaIPWhitelist>
    <kibanaDomain>es-cn-nif1q9o8r0008****.kibana.elasticsearch.aliyuncs.com</kibanaDomain>
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
        <name>dict_0.txt</name>
        <fileSize>220</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>SYNONYMS</type>
    </synonymsDicts>
    <haveGrafana>false</haveGrafana>
    <haveCerebro>false</haveCerebro>
    <enableKibanaPublicNetwork>true</enableKibanaPublicNetwork>
    <enableKibanaPrivateNetwork>false</enableKibanaPrivateNetwork>
    <advancedSetting>
        <gcName>CMS</gcName>
    </advancedSetting>
</Result>
<RequestId>17632473-6A3B-4D0C-A603-A56D5743****</RequestId>
```

`JSON` 格式

```
{
	"Result": {
		"instanceId": "es-cn-nif1q9o8r0008****",
		"version": "6.7.0_with_X-Pack",
		"description": "es-cn-nif1q9o8r0008****",
		"nodeAmount": 4,
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
		"createdAt": "2020-07-07T04:05:16.791Z",
		"updatedAt": "2020-07-07T10:15:14.498Z",
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
				"aliVersion": "ali1.2.0"
			}
		],
		"endTime": 4749811200000,
		"clusterTasks": [
			{
				"type": "MigrateData",
				"progress": 100,
				"detail": {},
				"status": "FINISHED",
				"canCancelable": false,
				"interruptible": false,
				"subTasks": [
					{
						"type": "FindShrinkNodeAction",
						"progress": 100,
						"detail": {},
						"status": "FINISHED",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					},
					{
						"type": "MigrateDataAction",
						"progress": 100,
						"detail": {
							"doneMigrateNodeIps": [
								"172.16.**.**"
							],
							"allMigrateNodeIps": [
								"172.16.**.**"
							]
						},
						"status": "FINISHED",
						"canCancelable": false,
						"interruptible": false,
						"subTasks": []
					}
				]
			}
		],
		"vpcInstanceId": "es-cn-nif1q9o8r0008****-worker",
		"resourceGroupId": "rg-acfm2h5vbzdokea",
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
		"domain": "es-cn-nif1q9o8r0008****.elasticsearch.aliyuncs.com",
		"port": 9200,
		"esVersion": "6.7.0_with_X-Pack",
		"esConfig": {
			"thread_pool.bulk.queue_size": "500"
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
		"kibanaDomain": "es-cn-nif1q9o8r0008****.kibana.elasticsearch.aliyuncs.com",
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
		"synonymsDicts": [
			{
				"name": "dict_0.txt",
				"fileSize": 220,
				"sourceType": "ORIGIN",
				"type": "SYNONYMS"
			}
		],
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
	"RequestId": "17632473-6A3B-4D0C-A603-A56D5743****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

