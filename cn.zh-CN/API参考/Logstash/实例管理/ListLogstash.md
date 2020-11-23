# ListLogstash

调用ListLogstash，在列表中展示所有或指定Logstash实例的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListLogstash&type=ROA&version=2017-06-13)

## 请求头

/openapi/logstashes

## 请求语法

```
GET /openapi/logstashes HTTPS|HTTP
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|page|Integer|Query|否|1|实例列表的页码。起始值：1，默认值：1。 |
|size|Integer|Query|否|10|分页查询时设置的每页条数。最大值：100，默认值：10。 |
|description|String|Query|否|ls-cn-abc|实例名称，支持模糊查询。例如查询名称为abc的实例，则可能返回名称为abc、abcde、xyabc、xabcy的所有实例。 |
|instanceId|String|Query|否|ls-cn-n6w1o5jq\*\*\*\*|实例ID。 |
|version|String|Query|否|5.5.3\_with\_X-Pack|实例版本。 |
|resourceGroupId|String|Query|否|rg-acfm2h5vbzd\*\*\*\*|资源组ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |请求头信息。 |
|X-Total-Count|Integer|10|实例总记录数。 |
|RequestId|String|AC442F2F-5068-4434-AA21-E78947A9\*\*\*\*|请求ID。 |
|Result|Array of Instance| |当前请求返回的实例列表。 |
|Tags|Array of tags| |实例标签。 |
|TagKey|String|env|标签键。 |
|TagValue|String|dev|标签值。 |
|createdAt|String|2018-07-13T03:58:07.253Z|实例创建时间。 |
|description|String|ls-cn-abc|实例名称。 |
|instanceId|String|ls-cn-n6w1o5jq\*\*\*\*|实例ID。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，目前仅支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-abc|专有网络ID。 |
|vsArea|String|cn-hangzhou-\*|实例所在的可用区。 |
|vswitchId|String|vsw-def|交换机ID。 |
|nodeAmount|Integer|2|实例的节点个数。 |
|nodeSpec|Struct| |数据节点的配置信息。 |
|disk|Integer|50|节点磁盘大小。 |
|diskEncryption|Boolean|false|是否使用磁盘加密：

 -   true：使用
-   false：不使用 |
|diskType|String|cloud\_ssd|磁盘类型。 |
|spec|String|logstash.n4.small|实例规格。 |
|paymentType|String|postpaid|实例的付费模式。支持：prepaid（包年包月）、postpaid（按量付费）。 |
|status|String|active|实例的状态。支持四种状态：正常（active）、生效中（activating）、冻结（inactive）和失效（invalid）。 |
|updatedAt|String|2018-07-18T10:10:04.484Z|实例最后更新的时间。 |
|version|String|6.7.0\_with\_X-Pack|实例版本。目前仅支持6.7.0\_with\_X-Pack、7.4.0\_with\_X-Pack。 |

返回数据中还包含以下参数。

|名称

|类型

|示例值

|描述 |
|----|----|-----|----|
|enablePublic

|Boolean

|false

|是否开启公网访问，默认为false。 |
|commodityCode

|String

|elasticsearch\_logstash\_post

|产品代码。 |
|endTime

|Long

|4749897600000

|包年包月实例，最后的失效时间。 |
|clusterTasks

|Array

|\[\]

|实例的任务列表。 |
|resourceGroupId

|String

|rg-acfm2h5vbzd\*\*\*\*

|实例所在的资源组ID。 |
|zoneCount

|Integer

|1

|实例的可用区个数。 |
|protocol

|String

|HTTP

|实例的访问协议。 |
|zoneInfos

|Array

| |可用区信息。 |
|└zoneId

|String

|cn-hangzhou-i

|可用区ID。 |
|└status

|String

|NORMAL

|可用区状态。支持：**ISOLATION**（下线）、**NORMAL**（正常）。 |
|instanceType

|String

|logstash

|实例类型。 |
|inited

|Boolean

|true

|实例是否已完成初始化。 |
|config

|Array

|\[\]

|实例配置。 |
|endpointList

|Array

| |节点信息。 |
|└host

|String

|172.16.xx.xx

|节点的IP地址。 |
|└port

|Integer

|9200

|节点的访问端口号。 |
|└zoneId

|String

|cn-hangzhou-i

|节点所在的可用区ID。 |

**说明：** └表示子参数。

## 示例

请求示例

```
GET /openapi/logstashes?description=abc&page=1&size=10
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>ls-cn-n6w1o5jq****</instanceId>
    <version>6.7.0_with_X-Pack</version>
    <description>test</description>
    <nodeAmount>1</nodeAmount>
    <paymentType>postpaid</paymentType>
    <status>active</status>
    <enablePublic>false</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.sn1ne.large</spec>
        <disk>20</disk>
        <diskType>cloud_ssd</diskType>
    </nodeSpec>
    <networkConfig>
        <vpcId>vpc-bp16k1dvzxtmagcva****</vpcId>
        <vswitchId>vsw-bp1k4ec6s7sjdbudw****</vswitchId>
        <vsArea>cn-hangzhou-i</vsArea>
        <type>vpc</type>
    </networkConfig>
    <createdAt>2020-05-27T01:30:15.947Z</createdAt>
    <updatedAt>2020-05-27T01:40:51.333Z</updatedAt>
    <commodityCode>elasticsearch_logstash_post</commodityCode>
    <endTime>4746268800000</endTime>
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-hangzhou-i</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>logstash</instanceType>
    <inited>true</inited>
    <config/>
    <endpointList>
        <host>172.16.**.**</host>
        <port>9600</port>
        <zoneId>cn-hangzhou-i</zoneId>
    </endpointList>
</Result>
<RequestId>918C05D8-4689-4A79-B6D5-D2500991****</RequestId>
<Headers>
    <X-Total-Count>1</X-Total-Count>
</Headers>
```

`JSON` 格式

```
{
	"Result": [
		{
			"instanceId": "ls-cn-n6w1o5jq****",
			"version": "6.7.0_with_X-Pack",
			"description": "test",
			"nodeAmount": 1,
			"paymentType": "postpaid",
			"status": "active",
			"enablePublic": false,
			"nodeSpec": {
				"spec": "elasticsearch.sn1ne.large",
				"disk": 20,
				"diskType": "cloud_ssd"
			},
			"networkConfig": {
				"vpcId": "vpc-bp16k1dvzxtmagcva****",
				"vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
				"vsArea": "cn-hangzhou-i",
				"type": "vpc"
			},
			"createdAt": "2020-05-27T01:30:15.947Z",
			"updatedAt": "2020-05-27T01:40:51.333Z",
			"commodityCode": "elasticsearch_logstash_post",
			"extendConfigs": [],
			"endTime": 4746268800000,
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
			"instanceType": "logstash",
			"inited": true,
			"tags": [],
			"config": {},
			"endpointList": [
				{
					"host": "172.16.**.**",
					"port": 9600,
					"zoneId": "cn-hangzhou-i"
				}
			]
		}
	],
	"RequestId": "918C05D8-4689-4A79-B6D5-D2500991****",
	"Headers": {
		"X-Total-Count": 1
	}
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

