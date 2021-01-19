# UpdateCollectorName

调用UpdateCollectorName，修改采集器名称。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateCollectorName&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/collectors/[ResId]/actions/rename HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ResId|String|Path|是|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

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
|collectorPaths|List|\["/var/log"\]|fileBeat采集路径。当采集器的部署机器为ECS时显示。 |
|configs|Array of configs| |采集器的配置文件信息。 |
|content|String|- key: log\\n title: Log file content\\n description: \>\\n Contains log file lines.\\n ....|文件内容。 |
|fileName|String|fields.yml|文件名。 |
|dryRun|Boolean|false|是否校验并创建采集器。支持：

 -   true：只校验不更新
-   false：校验并更新 |
|extendConfigs|Array of extendConfigs| |采集器扩展配置。 |
|configType|String|collectorDeployMachine|配置类型。支持：

 -   collectorTargetInstance：采集器Output
-   collectorDeployMachine：采集器的部署机器
-   collectorElasticsearchForKibana：支持Kibana仪表盘的Elasticsearch实例信息 |
|enableMonitoring|Boolean|true|是否启用Monitoring。当**configType**为**collectorTargetInstance**，且**instanceType**为**elasticsearch**时显示。支持：

 -   true：启用
-   false：不启用 |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|机器组ID。当**configType**为**collectorDeployMachine**时显示。 |
|host|String|es-cn-4591jumei000u\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的私网访问地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|采集器Output指定实例的访问地址列表。当**configType**为**collectorTargetInstance**时显示。 |
|instanceId|String|es-cn-n6w1o1\*\*\*\*|采集器关联的实例ID。当**configType**为**collectorTargetInstance**时，为采集器Output的实例ID；当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时，为ACK集群ID。 |
|instanceType|String|elasticsearch|采集器Output指定的实例类型，支持：elasticsearch、logstash。当**configType**为**collectorTargetInstance**时显示。 |
|kibanaHost|String|https://es-cn-4591jumei000u\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的公网访问地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|machines|Array of machines| |采集器所部署的ECS机器列表信息。当**configType**为**collectorDeployMachines**，且**type**为**ECSInstanceId**时显示。 |
|agentStatus|String|heartOk|ECS上各采集器的状态。支持：**heartOk**（心跳正常）、**heartLost**（心跳异常）、**uninstalled**（未安装）和**failed**（安装失败）。 |
|instanceId|String|c1b9fde5172b84f82b9928e825a7b8988|ECS机器ID列表。 |
|protocol|String|HTTP|传输协议，支持**HTTP**、**HTTPS**。 |
|successPodsCount|String|8|ACK集群所有成功采集的Pod节点数。当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时显示。 |
|totalPodsCount|String|10|ACK集群所有采集的Pod节点数。当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时显示。 |
|type|String|ECSInstanceId|采集器部署的机器类型，当**configType**为**collectorDeployMachine**时显示。支持：

 -   ECSInstanceId：ECS实例
-   ACKCluster：容器Kubernetes集群 |
|userName|String|elastic|采集器Output指定实例的访问用户名，默认为elastic。当**configType**为**collectorTargetInstance**或**collectorElasticsearchForKibana**时显示。 |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|采集器创建时间。 |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|采集器更新时间。 |
|name|String|ct-test|采集器名称。 |
|ownerId|String|16852099488\*\*\*\*\*|账号ID。 |
|resId|String|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器实例ID。 |
|resType|String|fileBeat|采集器类型，支持fileBeat、metricBeat、heartBeat和audiBeat。 |
|resVersion|String|6.8.5\_with\_community|采集器版本。支持的版本与采集器的部署机器类型相关，具体如下：

 -   ECS：6.8.5\_with\_community
-   ACK：6.8.13\_with\_community |
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

`XML`格式

```
<Result>
    <gmtCreatedTime>2021-01-14T08:54:42.000+0000</gmtCreatedTime>
    <gmtUpdateTime>2021-01-18T03:59:20.140+0000</gmtUpdateTime>
    <name>API_test</name>
    <resId>ct-cn-tfv81t7vs8608****</resId>
    <resVersion>6.8.5_with_community</resVersion>
    <vpcId>vpc-bp12nu14urf0upaf4****</vpcId>
    <resType>fileBeat</resType>
    <ownerId>168520994880****</ownerId>
    <collectorPaths>/opt/test/logs/</collectorPaths>
    <configs>
        <fileName>fields.yml</fileName>
        <content>- key: log
  title: Log file content
  description: &gt;
    Contains log file lines.
  fields:
 ......</content>
    </configs>
    <configs>
        <fileName>filebeat.yml</fileName>
        <content>###################### Filebeat Configuration Example #########################

# This file is an example configuration file ......</content>
    </configs>
    <status>active</status>
    <extendConfigs>
        <configType>collectorTargetInstance</configType>
        <instanceId>es-cn-nif201ihd0012****</instanceId>
        <instanceType>elasticsearch</instanceType>
        <hosts>es-cn-nif201ihd0012****.elasticsearch.aliyuncs.com:9200</hosts>
        <protocol>HTTP</protocol>
        <userName>elastic</userName>
        <enableMonitoring>false</enableMonitoring>
    </extendConfigs>
    <extendConfigs>
        <configType>collectorDeployMachine</configType>
        <type>ECSInstanceId</type>
        <machines>
            <instanceId>i-bp11u91xgubypcuz****</instanceId>
            <agentStatus>heartOk</agentStatus>
        </machines>
        <groupId>default_ct-cn-tfv81t7vs8608****</groupId>
    </extendConfigs>
    <dryRun>false</dryRun>
</Result>
<RequestId>9B2BD604-3B93-4F66-91F0-43B4D2D268FF</RequestId>
```

`JSON`格式

```
{
	"Result": {
		"gmtCreatedTime": "2021-01-14T08:54:42.000+0000",
		"gmtUpdateTime": "2021-01-18T03:59:20.140+0000",
		"name": "API_test",
		"resId": "ct-cn-tfv81t7vs8608****",
		"resVersion": "6.8.5_with_community",
		"vpcId": "vpc-bp12nu14urf0upaf4****",
		"resType": "fileBeat",
		"ownerId": "168520994880****",
		"collectorPaths": [
			"/opt/test/logs/"
		],
		"configs": [
			{
				"fileName": "fields.yml",
				"content": "- key: log\n  title: Log file content\n  description: >\n    Contains log file lines.\n  fields:\n ......"    
			},
			{
				"fileName": "filebeat.yml",
				"content": "###################### Filebeat Configuration Example #########################\n\n# This file is an example configuration file ......"
			}
		],
		"status": "active",
		"extendConfigs": [
			{
				"configType": "collectorTargetInstance",
				"instanceId": "es-cn-nif201ihd0012****",
				"instanceType": "elasticsearch",
				"hosts": [
					"es-cn-nif201ihd0012****.elasticsearch.aliyuncs.com:9200"
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
						"instanceId": "i-bp11u91xgubypcuz****",
						"agentStatus": "heartOk"
					}
				],
				"groupId": "default_ct-cn-tfv81t7vs8608****"
			}
		],
		"dryRun": false
	},
	"RequestId": "9B2BD604-3B93-4F66-91F0-43B4D2D268FF"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

