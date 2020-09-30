# UpdateLogstashSettings

调用UpdateLogstashSettings，更新指定Logstash实例的配置。

调用该接口时，请注意：

实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法更新信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateLogstashSettings&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/logstashes/[InstanceId]/instance-settings HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|ls-cn-oew1qbgl\*\*\*\*|实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|BB1C321A-211C-4FD7-BD8B-7F2FABE2\*\*\*\*|请求ID。 |

返回数据中还包含Result参数，参数说明请参见[ListLogstash](~~160534~~)。

## 示例

请求示例

```
PATCH /openapi/logstashes/ls-cn-oew1qbgl****/instance-settings HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <instanceId>ls-cn-oew1qbgl****</instanceId>
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
    <createdAt>2020-07-08T03:15:27.218Z</createdAt>
    <updatedAt>2020-07-08T03:26:01.931Z</updatedAt>
    <commodityCode>elasticsearch_logstash_post</commodityCode>
    <endTime>4749897600000</endTime>
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
<RequestId>629D3748-5DB5-495F-BCAA-6EA240DA****</RequestId>
```

`JSON` 格式

```
{
	"Result": {
		"instanceId": "ls-cn-oew1qbgl****",
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
		"createdAt": "2020-07-08T03:15:27.218Z",
		"updatedAt": "2020-07-08T03:26:01.931Z",
		"commodityCode": "elasticsearch_logstash_post",
		"extendConfigs": [],
		"endTime": 4749897600000,
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
	},
	"RequestId": "629D3748-5DB5-495F-BCAA-6EA240DA****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

