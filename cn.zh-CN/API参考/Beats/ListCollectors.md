# ListCollectors

调用ListCollectors，获取采集器列表信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListCollectors&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/collectors HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|resId|String|是|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器ID。 |
|name|String|否|collectorName1|采集器名称。 |
|instanceId|String|否|es-cn-nif1q8auz0003\*\*\*\*|采集器Output指定的Elasticsearch实例ID。 |
|page|Integer|否|1|返回结果的分页数。 |
|size|Integer|否|10|每页的结果数。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|5|返回的记录数。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|collectorPaths|List|\["/var/log"\]|Filebeat的采集路径。 |
|configs|Array of configs| |采集器的配置文件信息。 |
|content|String|- key: log\\n title: Log file content\\n description: \>\\n Contains log file lines.\\n ....|文件内容。 |
|fileName|String|fields.yml|文件名称。 |
|dryRun|Boolean|false|是否校验并创建采集器，只有在创建或更新采集器时使用此参数。true表示只校验不创建，false表示校验并创建。 |
|extendConfigs|Array of extendConfigs| |扩展参数信息。 |
|configType|String|collectorDeployMachine|配置类型。可选值：

 -   Beats输出目标：collectorTargetInstance
-   Beats的安装机器：collectorDeployMachine
-   支持Kibana仪表盘的Elasticsearch实例信息：collectorElasticsearchForKibana |
|enableMonitoring|Boolean|true|为采集器启用Monitoring。该参数是configType为collectorElasticsearchForKibana时的专有参数。 |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|该参数是configType为collectorDeployMachine时的专有参数。 |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|采集器输出实例的地址。该参数是configType为collectorTargetInstance时的专有参数。 |
|instanceId|String|es-cn-abc|采集器输出的实例ID。 |
|instanceType|String|elasticsearch|采集器输出的实例类型，支持elasticsearch和logstash。该参数为configType为collectorTargetInstance时的专有参数。 |
|machines|Array of machines| |安装采集器的ECS机器。该参数是configType为collectorDeployMachine时的专有参数。 |
|agentStatus|String|heartOk或failed|ECS上各采集器的状态。支持：heartOk（心跳正常）、heartLost（心跳异常）、uninstalled（未安装）和failed（安装失败）。 |
|instanceId|String|succeed或failed|ECS机器ID列表。 |
|protocol|String|HTTP|传输协议。 |
|type|String|ECSInstanceId|安装采集器的机器类型。目前仅支持ECS实例，对应的参数值为ECSInstanceId。 |
|userName|String|elastic|采集器output为Elasticsearch时，对应实例的访问用户名，默认为elastic。 |
|gmtCreatedTime|String|2020-08-18T02:06:12.000+0000|采集器创建时间。 |
|gmtUpdateTime|String|2020-08-18T09:40:43.000+0000|采集器更新时间。 |
|name|String|FileBeat001|采集器名称。 |
|ownerId|String|168520994880\*\*\*\*|账号ID。 |
|resId|String|ct-cn-0v3xj86085dvq\*\*\*\*|采集器实例ID。 |
|resType|String|fileBeat|采集器类型，支持fileBeat、metricBeat、heartBeat和audiBeat。 |
|resVersion|String|6.8.5\_with\_community|采集器版本。 |
|status|String|active|采集器状态。支持activing（生效中）和active（已生效）。 |
|vpcId|String|vpc-bp16k1dvzxtma\*\*\*\*\*|采集器所在的专有网络ID。 |

未配置的参数不会显示在返回结果中。

## 示例

请求示例

```
GET /openapi/collectors?resId=ct-cn-77uqof2s7rg5c**** HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>
    <gmtCreatedTime>2020-08-19T08:59:00.000+0000</gmtCreatedTime>
    <gmtUpdateTime>2020-09-21T09:49:01.000+0000</gmtUpdateTime>
    <name>collectorName1</name>
    <resId>ct-cn-77uqof2s7rg5c****</resId>
    <resVersion>6.8.5_with_community</resVersion>
    <vpcId>vpc-bp16k1dvzxtmagcva****</vpcId>
    <resType>fileBeat</resType>
    <ownerId>168520994880****</ownerId>
    <collectorPaths>/opt/var/logs</collectorPaths>
    <configs>
        <fileName>fields.yml</fileName>
    </configs>
    <configs>
        <fileName>filebeat.yml</fileName>
    </configs>
    <status>active</status>
    <extendConfigs>
        <configType>collectorTargetInstance</configType>
        <instanceId>es-cn-nif1q8auz0003****</instanceId>
        <instanceType>elasticsearch</instanceType>
        <hosts>es-cn-nif1q8auz0003****.elasticsearch.aliyuncs.com:9200</hosts>
        <protocol>HTTP</protocol>
        <userName>elastic</userName>
        <enableMonitoring>false</enableMonitoring>
    </extendConfigs>
    <extendConfigs>
        <configType>collectorDeployMachine</configType>
        <type>ECSInstanceId</type>
        <machines>
            <instanceId>i-bp1gyhphjaj73jsr****</instanceId>
            <agentStatus>heartOk</agentStatus>
        </machines>
        <machines>
            <instanceId>i-bp10piq1mkfnyw9t****</instanceId>
            <agentStatus>failed</agentStatus>
        </machines>
        <groupId>default_ct-cn-77uqof2s7rg5c****</groupId>
    </extendConfigs>
    <dryRun>false</dryRun>
</Result>
<RequestId>D8960B17-3C29-43A6-8A8C-0B1DEB13****</RequestId>
<Headers>
    <X-Total-Count>1</X-Total-Count>
</Headers>
```

`JSON` 格式

```
{
	"Result": [
		{
			"gmtCreatedTime": "2020-08-19T08:59:00.000+0000",
			"gmtUpdateTime": "2020-09-21T09:49:01.000+0000",
			"name": "collectorName1",
			"resId": "ct-cn-77uqof2s7rg5c****",
			"resVersion": "6.8.5_with_community",
			"vpcId": "vpc-bp16k1dvzxtmagcva****",
			"resType": "fileBeat",
			"ownerId": "168520994880****",
			"collectorPaths": [
				"/opt/var/logs"
			],
			"configs": [
				{
					"fileName": "fields.yml"
				},
				{
					"fileName": "filebeat.yml"
				}
			],
			"status": "active",
			"extendConfigs": [
				{
					"configType": "collectorTargetInstance",
					"instanceId": "es-cn-nif1q8auz0003****",
					"instanceType": "elasticsearch",
					"hosts": [
						"es-cn-nif1q8auz0003****.elasticsearch.aliyuncs.com:9200"
					],
					"protocol": "HTTP",
					"userName": "elastic",
					"enableMonitoring": false
				},
				{
					"configType": "collectorDeployMachine",
					"type": "ECSInstanceId",
					"machines": [
						{
							"instanceId": "i-bp1gyhphjaj73jsr****",
							"agentStatus": "heartOk"
						},
						{
							"instanceId": "i-bp10piq1mkfnyw9t****",
							"agentStatus": "failed"
						}
					],
					"groupId": "default_ct-cn-77uqof2s7rg5c****"
				}
			],
			"dryRun": false
		}
	],
	"RequestId": "D8960B17-3C29-43A6-8A8C-0B1DEB13****",
	"Headers": {
		"X-Total-Count": 1
	}
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

