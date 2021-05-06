# ListEcsInstances

调用ListEcsInstances，获取ECS机器列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListEcsInstances&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/ecs HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|page|Integer|Query|否|1|返回结果页数。 |
|size|Integer|Query|否|10|每页包含的结果数。 |
|ecsInstanceIds|String|Query|否|\["i-bp13y63575oypr9d\*\*\*\*","i-bp1gyhphjaj73jsr\*\*\*\*"\]|ECS实例ID列表。取值可以由多个实例ID组成一个JSON数组，最多支持100个ID，ID之间用半角逗号（,）隔开。 |
|ecsInstanceName|String|Query|否|test|ECS实例名称。 |
|tags|String|Query|否|\[\{ "tagKey":"a","tagValue":"b"\}\]|ECS实例标签，必须包含：

 -   tagKey：标签键
-   tagValue：标签值 |
|vpcId|String|Query|否|vpc-bp16k1dvzxtmagcva\*\*\*\*|ECS实例所在的VPC ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|11|返回的记录数。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|cloudAssistantStatus|String|true|云助手安装状态，支持：

 -   true：已安装
-   false：未安装 |
|collectors|Array of collectors| |该ECS实例上，采集器信息列表。 |
|collectorPaths|List|\["/var/log"\]|Filebeat的采集路径。 |
|configs|Array of configs| |采集器的配置文件信息。 |
|content|String|- key: log\\n title: Log file content\\n description: \>\\n Contains log file lines.\\n ....|文件内容。 |
|fileName|String|fields.yml|文件名称。 |
|dryRun|Boolean|false|是否校验并创建采集器。支持：

 -   true：只校验不创建
-   false：校验并创建 |
|extendConfigs|Array of extendConfigs| |扩展参数信息。 |
|configType|String|collectorDeployMachine|配置类型。支持：

 -   collectorTargetInstance：采集器Output
-   collectorDeployMachine：采集器的部署机器
-   collectorElasticsearchForKibana：支持Kibana仪表盘的Elasticsearch实例信息 |
|enableMonitoring|Boolean|true|是否启用Monitoring，当**configType**为**collectorTargetInstance**，且**instanceType**为**elasticsearch**时显示。支持：

 -   true：启用
-   false：不启用 |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|机器组ID。当**configType**为**collectorDeployMachine**时显示。 |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|采集器Output指定实例的访问地址列表。当**configType**为**collectorTargetInstance**时显示。 |
|instanceId|String|es-cn-nif1z89fz003i\*\*\*\*|采集器关联的实例ID。当**configType**为**collectorTargetInstance**时，为采集器Output的实例ID；当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时，为ACK（容器Kubernetes）集群ID。 |
|instanceType|String|elasticsearch|采集器Output指定的实例类型。支持elasticsearch、logstash。当**configType**为**collectorTargetInstance**时显示。 |
|machines|Array of machines| |采集器所部署的ECS机器列表信息。当**configType**为**collectorDeployMachines**，且**type**为**ECSInstanceId**时显示。 |
|agentStatus|String|heartOk或failed|ECS上各采集器的状态。支持：

 -   heartOk：心跳正常
-   heartLost：心跳异常
-   uninstalled：未安装
-   failed：安装失败 |
|instanceId|String|i-bp13y63575oypr9d\*\*\*\*|ECS机器ID列表。 |
|protocol|String|HTTP|传输协议，需要与采集器Output指定实例的访问协议保持一致。支持HTTP、HTTPS。当**configType**为**collectorTargetInstance**时显示。 |
|type|String|ECSInstanceId|采集器部署的机器类型，当**configType**为**collectorDeployMachine**时显示。支持：

 -   ECSInstanceId：ECS
-   ACKCluster：容器Kubernetes |
|userName|String|elastic|采集器Output指定实例的访问用户名，默认为elastic。当**configType**为**collectorTargetInstance**或**collectorElasticsearchForKibana**时显示。 |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|采集器创建时间。 |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|采集器更新时间。 |
|name|String|ct-testAbc|采集器名称。 |
|ownerId|String|16852\*\*\*488\*\*\*\*\*|账号ID。 |
|resId|String|ct-cn-0v3xj86085dvq\*\*\*\*|采集器实例ID。 |
|resType|String|fileBeat|采集器类型。支持fileBeat、metricBeat、heartBeat和auditBeat。 |
|resVersion|String|6.8.5\_with\_community|采集器版本。采集器部署的机器类型为ECS时，只支持**6.8.5\_with\_community**。 |
|status|String|activing|采集器状态。支持：

 -   activing：生效中
-   active：已生效 |
|vpcId|String|vpc-bp16k1dvzxtm\*\*\*\*\*\*|采集器所在的专有网络ID。 |
|ecsInstanceId|String|i-bp14ncqge8wy3l3d\*\*\*\*|ECS实例ID。 |
|ecsInstanceName|String|ecsTestName|ECS实例名称。 |
|ipAddress|Array of ipAddress| |ECS实例的IP地址信息。 |
|host|String|172.16.xx.xx|IP地址。 |
|ipType|String|private|IP地址类型。支持：

 -   public：公网地址
-   private：私网地址 |
|osType|String|linux|ECS实例的操作系统类型。支持：

 -   windows：Windows操作系统
-   linux：Linux操作系统 |
|status|String|running|ECS实例的状态。支持：

 -   running：运行中
-   starting：启动中
-   stopping：停止中
-   stopped：已停止 |
|tags|String|\[ \{ "tagKey": "a", "tagValue": "b" \} \]|ECS实例的标签信息。 |

## 示例

请求示例

```
GET /openapi/ecs?page=1&size=10 HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"ecsInstanceId": "i-bp1gyhphjaj73jsr****",
			"ecsInstanceName": "test",
			"status": "running",
			"ipAddress": [
				{
					"host": "47.98.xx.xx",
					"ipType": "public"
				},
				{
					"host": "172.16.xx.xx",
					"ipType": "private"
				}
			],
			"tags": [],
			"collectors": [
				{
					"gmtCreatedTime": "2020-12-30T08:04:32.000+0000",
					"gmtUpdateTime": "2020-12-30T08:20:48.000+0000",
					"name": "uptime-test",
					"resId": "ct-cn-4135is2tj194p****",
					"resVersion": "6.8.5_with_community",
					"vpcId": "vpc-bp16k1dvzxtmagcva****",
					"resType": "heartBeat",
					"ownerId": "168520994880****",
					"configs": [
						{
							"fileName": "fields.yml"
						},
						{
							"fileName": "heartbeat.yml"
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
							"enableMonitoring": true
						},
						{
							"configType": "collectorDeployMachine",
							"type": "ECSInstanceId",
							"machines": [
								{
									"instanceId": "i-bp1gyhphjaj73jsr****",
									"agentStatus": "heartOk"
								}
							],
							"groupId": "default_ct-cn-4135is2tj194p****"
						}
					],
					"dryRun": false
				}
			],
			"osType": "linux",
			"cloudAssistantStatus": "true"
		}
	],
	"RequestId": "58E5DE98-33B0-4D9B-B5F6-E70A77C5933E",
	"Headers": {
		"X-Total-Count": 2
	}
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

