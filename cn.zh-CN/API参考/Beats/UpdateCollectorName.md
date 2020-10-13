# UpdateCollectorName

调用UpdateCollectorName，修改采集器名称。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateCollectorName&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/collectors/[ResId]/actions/rename HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ResId|String|是|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器ID。 |
|ClientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定修改后的采集器名称。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|name

|String

|是

|collectorName1

|修改后的采集器名称。 |

示例如下。

```

{
  "name": "collectorName1"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|collectorPaths|List|\["/var/log"\]|Filebeat的采集路径。 |
|configs|Array of configs| |采集器的配置文件信息。 |
|content|String|- key: log\\n title: Log file content\\n description: \>\\n Contains log file lines.\\n ....|文件内容。 |
|fileName|String|filebeat.yml|文件名称。 |
|dryRun|Boolean|false|是否校验并创建采集器，只有在创建或更新采集器时使用此参数。true表示只校验不创建，false表示校验并创建。 |
|extendConfigs|Array of extendConfigs| |扩展参数信息。 |
|configType|String|collectorDeployMachine|配置类型。可选值：

 -   Beats输出目标：collectorTargetInstance
-   Beats的安装机器：collectorDeployMachine
-   支持Kibana仪表盘的Elasticsearch实例信息：collectorElasticsearchForKibana |
|enableMonitoring|Boolean|true|为采集器启用Monitoring。该参数是**configType**为**collectorElasticsearchForKibana**时的专有参数。 |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|该参数是**configType**为**collectorDeployMachine**时的专有参数。 |
|host|String|es-cn-4591jumei000u\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601|Kibana的内网访问地址。该参数是**configType**为**collectorElasticsearchForKibana**时的专有参数。 |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|采集器输出实例的地址。该参数是**configType**为**collectorTargetInstance**时的专有参数。 |
|instanceId|String|es-cn-n6w1o1\*\*\*\*|采集器输出的实例ID。 |
|instanceType|String|elasticsearch|采集器输出的实例类型，支持elasticsearch和logstash。该参数为**configType**为**collectorTargetInstance**时的专有参数。 |
|kibanaHost|String|https://es-cn-4591jumei000u\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|Kibana的公网访问地址。该参数是**configType**为**collectorElasticsearchForKibana**时的专有参数。 |
|machines|Array of machines| |安装采集器的ECS机器。该参数是**configType**为**collectorDeployMachine**时的专有参数。 |
|agentStatus|String|heartOk|ECS上各采集器的状态。支持：heartOk（心跳正常）、heartLost（心跳异常）、uninstalled（未安装）和failed（安装失败）。 |
|instanceId|String|c1b9fde5172b84f82b9928e825a7b8988|ECS机器ID列表。 |
|protocol|String|HTTP或HTTPS|传输协议。 |
|type|String|ECSInstanceId|安装采集器的机器类型，目前仅支持ECS实例，对应的值为**ECSInstanceId**。该参数是**configType**为**collectorDeployMachine**时的专有参数。 |
|userName|String|elastic|采集器output为Elasticsearch时，对应实例的访问用户名，默认为elastic。 |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|采集器创建时间。 |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|采集器更新时间。 |
|name|String|ct-test|采集器名称。 |
|ownerId|String|16852099488\*\*\*\*\*|账号ID。 |
|resId|String|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器实例ID。 |
|resType|String|fileBeat|采集器类型，支持fileBeat、metricBeat、heartBeat和audiBeat。 |
|resVersion|String|6.8.5\_with\_community|采集器版本。 |
|status|String|active|采集器状态。支持activing（生效中）和active（已生效）。 |
|vpcId|String|vpc-bp16k1dvzxtma\*\*\*\*\*|采集器所在的专有网络ID。 |

## 示例

请求示例

```
POST /openapi/collectors/ct-cn-77uqof2s7rg5c****/actions/rename HTTP/1.1
公共请求头
{
  "name": "collectorName1"
}
```

正常返回示例

`XML` 格式

```
<Result>
    <gmtCreatedTime>2020-09-17T07:36:30.000+0000</gmtCreatedTime>
    <gmtUpdateTime>2020-09-17T07:36:30.000+0000</gmtUpdateTime>
    <name>test</name>
    <resId>ct-cn-rg31ahn82m0qd****</resId>
    <resVersion>6.8.5_with_community</resVersion>
    <vpcId>vpc-bp16k1dvzxtmagcva****</vpcId>
    <resType>fileBeat</resType>
    <ownerId>168xxxxxxxxx</ownerId>
    <collectorPaths>/var/log/*.log</collectorPaths>
    <configs>
        <fileName>fields.yml</fileName>
        <content>- key: log
  title: Log file content
  description: &gt;
    Contains log file lines.
 ....</content>
    </configs>
    <configs>
        <fileName>filebeat.yml</fileName>
        <content>###################### Filebeat Configuration Example #########################</content>
    </configs>
    <status>active</status>
    <extendConfigs>
        <configType>collectorTargetInstance</configType>
        <instanceId>es-cn-n6w1o1x0w001c****</instanceId>
        <instanceType>elasticsearch</instanceType>
        <hosts>es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200</hosts>
        <protocol>HTTP</protocol>
        <userName>elastic</userName>
        <enableMonitoring>false</enableMonitoring>
    </extendConfigs>
    <extendConfigs>
        <configType>collectorElasticsearchForKibana</configType>
        <instanceId>es-cn-4591jumei000u****</instanceId>
        <host>es-cn-4591jumei000u****-kibana.internal.elasticsearch.aliyuncs.com:5601</host>
        <protocol>HTTPS</protocol>
        <kibanaHost>https://es-cn-4591jumei000u****.kibana.elasticsearch.aliyuncs.com:5601</kibanaHost>
        <userName>elastic</userName>
    </extendConfigs>
    <extendConfigs>
        <configType>collectorDeployMachine</configType>
        <type>ECSInstanceId</type>
        <machines>
            <instanceId>i-bp1gyhphjaj73jsr****</instanceId>
            <agentStatus>failed</agentStatus>
        </machines>
        <groupId>default_ct-cn-rg31ahn82m0qd****</groupId>
    </extendConfigs>
    <dryRun>false</dryRun>
</Result>
<RequestId>7BDB122E-BD68-4F94-9A45-D8D216CD****</RequestId>
```

`JSON` 格式

```
{
	"Result": {
		"gmtCreatedTime": "2020-09-17T07:36:30.000+0000",
		"gmtUpdateTime": "2020-09-17T07:36:30.000+0000",
		"name": "test",
		"resId": "ct-cn-rg31ahn82m0qd****",
		"resVersion": "6.8.5_with_community",
		"vpcId": "vpc-bp16k1dvzxtmagcva****",
		"resType": "fileBeat",
		"ownerId": "168xxxxxxxxx",
		"collectorPaths": [
			"/var/log/*.log"
		],
		"configs": [
			{
				"fileName": "fields.yml",
				"content": "- key: log\n  title: Log file content\n  description: >\n    Contains log file lines.\n ...."
			},
			{
				"fileName": "filebeat.yml",
				"content": "###################### Filebeat Configuration Example #########################"
			}
		],
		"status": "active",
		"extendConfigs": [
			{
				"configType": "collectorTargetInstance",
				"instanceId": "es-cn-n6w1o1x0w001c****",
				"instanceType": "elasticsearch",
				"hosts": [
					"es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
				],
				"protocol": "HTTP",
				"userName": "elastic",
				"enableMonitoring": false
			},
			{
				"configType": "collectorElasticsearchForKibana",
				"instanceId": "es-cn-4591jumei000u****",
				"host": "es-cn-4591jumei000u****-kibana.internal.elasticsearch.aliyuncs.com:5601",
				"protocol": "HTTPS",
				"kibanaHost": "https://es-cn-4591jumei000u****.kibana.elasticsearch.aliyuncs.com:5601",
				"userName": "elastic"
			},
			{
				"configType": "collectorDeployMachine",
				"type": "ECSInstanceId",
				"machines": [
					{
						"instanceId": "i-bp1gyhphjaj73jsr****",
						"agentStatus": "failed"
					}
				],
				"groupId": "default_ct-cn-rg31ahn82m0qd****"
			}
		],
		"dryRun": false
	},
	"RequestId": "7BDB122E-BD68-4F94-9A45-D8D216CD****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

