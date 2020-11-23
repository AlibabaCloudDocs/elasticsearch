# DescribeLogstash

调用DescribeLogstash，查询指定实例的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeLogstash&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/logstashes/[InstanceId] HTTPS|HTTP
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-s9dsk3k4k\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|C9334241-4837-46C2-B24B-9BDC517318DE|请求ID。 |
|Result|Struct| |当前实例的详细信息。 |
|ExtendConfigs|List|\[\{ "configType": "aliVersion","aliVersion": "ali1.3.0" \}\]|集群扩展参数配置。 |
|ResourceGroupId|String|rg-aekzvowej3i\*\*\*\*|实例所属的资源组ID。 |
|Tags|Array of tags| |实例标签。 |
|tagKey|String|env|标签键。 |
|tagValue|String|dev|标签值。 |
|ZoneInfos|Array of zoneInfos| |可用区信息。 |
|status|String|NORMAL|可用区状态。支持：

 -   ISOLATION：下线
-   NORMAL：正常 |
|zoneId|String|cn-hangzhou-b|可用区ID。 |
|config|Map|\{"slowlog.threshold.warn": "2s","slowlog.threshold.info": "1s","slowlog.threshold.debug": "500ms","slowlog.threshold.trace": "100ms" \}|实例配置信息。 |
|createdAt|String|2020-02-06T14:12:03.672Z|实例创建时间。 |
|description|String|ls-cn-abc|实例描述。 |
|endpointList|Array of endpoint| |节点的访问信息。 |
|host|String|172.16.\*\*.\*\*|节点的IP地址。 |
|port|String|9600|端口号。 |
|zoneId|String|cn-hangzhou-b|节点所在的可用区ID。 |
|instanceId|String|ls-cn-abc|实例ID。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型。目前只支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-bp16k1dvzxtmagcva\*\*\*\*|专有网络ID。 |
|vsArea|String|cn-hangzhou-\*|实例所在的可用区。 |
|vswitchId|String|vsw-bp1k4ec6s7sjdbudw\*\*\*\*|虚拟交换机ID。 |
|nodeAmount|Integer|2|实例的节点个数。 |
|nodeSpec|Struct| |节点的配置信息。 |
|disk|Integer|20|节点的磁盘大小。 |
|diskEncryption|Boolean|true|是否使用云盘加密：

 -   true：使用
-   false：不使用 |
|diskType|String|cloud\_ssd|节点的磁盘类型。 |
|spec|String|elasticsearch.sn1ne.large|节点的规格。 |
|paymentType|String|prepaid|实例的付费模式。支持：prepaid（包年包月）、postpaid（按量付费）。 |
|status|String|active|实例的状态。支持四种状态：正常（active）、生效中（activating）、冻结（inactive）和失效（invalid）。 |
|updatedAt|String|2020-02-06T14:22:36.850Z|实例最后更新的时间。 |
|version|String|7.4.0\_with\_X-Pack|实例版本。 |
|vpcInstanceId|String|vpc-bp16k1dvzxtmagcva\*\*\*\*|实例所属的VPC ID。 |

更多参数说明请参见[ListLogstash](~~160534~~)。

## 示例

请求示例

```
GET /openapi/logstashes/ls-cn-s9dsk3k4k**** HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>ls-cn-m7r1o6cl****</instanceId>
    <version>6.7.0_with_X-Pack</version>
    <description>ls-cn-abc</description>
    <nodeAmount>2</nodeAmount>
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
    <createdAt>2020-05-27T11:53:43.104Z</createdAt>
    <updatedAt>2020-05-27T11:53:43.104Z</updatedAt>
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
    <config>
        <slowlog.threshold.warn>2s</slowlog.threshold.warn>
        <slowlog.threshold.info>1s</slowlog.threshold.info>
        <slowlog.threshold.debug>500ms</slowlog.threshold.debug>
        <slowlog.threshold.trace>100ms</slowlog.threshold.trace>
    </config>
    <endpointList>
        <host>172.16.**.**</host>
        <port>9600</port>
        <zoneId>cn-hangzhou-i</zoneId>
    </endpointList>
    <endpointList>
        <host>172.16.**.**</host>
        <port>9600</port>
        <zoneId>cn-hangzhou-i</zoneId>
    </endpointList>
</Result>
<RequestId>C9334241-4837-46C2-B24B-9BDC517318DE</RequestId>
```

`JSON` 格式

```
{
	"Result": {
		"instanceId": "ls-cn-m7r1o6cl****",
		"version": "6.7.0_with_X-Pack",
		"description": "ls-cn-abc",
		"nodeAmount": 2,
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
		"createdAt": "2020-05-27T11:53:43.104Z",
		"updatedAt": "2020-05-27T11:53:43.104Z",
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
		"config": {
			"slowlog.threshold.warn": "2s",
			"slowlog.threshold.info": "1s",
			"slowlog.threshold.debug": "500ms",
			"slowlog.threshold.trace": "100ms"
		},
		"endpointList": [
			{
				"host": "172.16.**.**",
				"port": 9600,
				"zoneId": "cn-hangzhou-i"
			},
			{
				"host": "172.16.**.**",
				"port": 9600,
				"zoneId": "cn-hangzhou-i"
			}
		]
	},
	"RequestId": "C9334241-4837-46C2-B24B-9BDC517318DE"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

