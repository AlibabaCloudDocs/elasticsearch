# CreateCollector

调用CreateCollector，创建采集器。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=CreateCollector&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/collectors HTTPS|HTTP
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定待创建采集器的配置信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|dryRun

|Boolean

|是

|true

|是否校验并创建采集器，只有在创建或更新采集器时使用此参数。可选值：true（只校验不更新）、false（校验并更新）。 |
|name

|String

|是

|ct-test

|采集器名称。 |
|resType

|String

|是

|fileBeat

|采集器类型。可选值：fileBeat、metricBeat、heartBeat和auditBeat。 |
|resVersion

|String

|是

|6.8.5\_with\_community

|采集器版本。 |
|vpcId

|Integer

|是

|vpc-bp12nu14urf0upaf\*\*\*\*\*

|采集器所在的专有网络ID。 |
|collectorPaths

|List<String\>

|否

|\["/var/log"\]

|fileBeat采集路径。仅当采集器的安装机器为ECS时，需要配置。 |
|configs

|List

|是

| |采集器的配置文件信息。 |
|└fileName

|String

|是

|filebeat.yml

|文件名。 |
|└content

|String

|是

|"filebeat.inputs:xxx"

|文件内容。 |
|extendConfigs

|Array

| | |采集器扩展配置。 |
|└configType

|String

|是

|collectorElasticsearchForKibana

|配置类型。可选值：collectorTargetInstance（采集器Output）、collectorDeployMachine（采集器的部署机器）、collectorElasticsearchForKibana（支持Kibana仪表盘的Elasticsearch实例信息）。 |
|└type

|String

|否

|ECSInstanceId

|采集器部署的机器类型。可选值：ECSInstanceId（ECS）、ACKCluster（容器Kubernetes）。当**configType**为**collectorDeployMachine**时必填。 |
|└instanceType

|String

|否

|elasticsearch

|采集器Output指定的实例类型。可选值：elasticsearch、logstash。当**configType**为**collectorTargetInstance**时必填。 |
|└instanceId

|String

|是

|es-cn-nif201ihd0012\*\*\*\*

|采集器关联的实例ID。当**configType**为**collectorTargetInstance**时，为采集器Output的实例ID；当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时，为ACK集群ID。 |
|└machines

|Array

|否

| |采集器所部署的ECS机器列表信息。当**configType**为**collectorDeployMachines**，且**type**为**ECSInstanceId**时，必填。 |
|└└instanceId

|String

|是

|i-bp11u91xgubypcuz\*\*\*\*

|ECS机器ID列表。 |
|└groupId

|String

| |default\_ct-cn-5i2l75bz4776\*\*\*\*

|机器组ID。当**configType**为**collectorDeployMachine**时，必填。 |
|└protocol

|String

|否

|HTTP

|传输协议，需要与采集器Output指定实例的访问协议保持一致。可选值：HTTP、HTTPS。当**configType**为**collectorTargetInstance**时必填。 |
|└userName

|String

|否

|elastic

|采集器Output指定实例的访问用户名，默认为elastic。当**configType**为**collectorTargetInstance**或**collectorElasticsearchForKibana**时必填。 |
|└password

|String

|否

|\*\*\*\*\*

|对应用户名的密码。 |
|└enableMonitoring

|Boolean

|否

|true

|是否启用Monitoring，当**configType**为**collectorTargetInstance**，且**instanceType**为**elasticsearch**时必填。可选值：true（启用）、false（不启用）。 |
|└hosts

|List<String\>

|否

|\["es-cn-nif201i\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]

|采集器Output指定实例的访问地址列表。当**configType**为**collectorTargetInstance**时必填。 |
|└host

|String

|否

|es-cn-nif201ihd0012\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601

|启用Kibana Dashboard后，Kibana的私网访问地址。当**configType**为**collectorElasticsearchForKibana**时必填。 |
|└kibanaHost

|String

|否

|https://es-cn-nif201ihd0012\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601

|启用Kibana Dashboard后，Kibana的公网访问地址。当**configType**为**collectorElasticsearchForKibana**时必填。 |

**说明：** └表示子参数。

## 特殊参数说明

**extendConfigs**中包括3种configType，分别为collectorTargetInstance、collectorElasticsearchForKibana、collectorDeployMachine，部署机器不同，需要配置的参数不同，具体组合方式如下：

-   collectorTargetInstance
    -   ECS

        configType、instanceId、instanceType、hosts、userName、password、protocol、enableMonitoring

    -   ACK

        configType、instanceId、instanceType、userName、password、protocol、enableMonitoring

-   collectorElasticsearchForKibana
    -   ECS

        configType、instanceId、host、kibanaHost、userName、password、protocol

    -   ACK

        configType

-   collectorDeployMachine
    -   ECS

        configType、type、machines、groupId

    -   ACK

        configType、type、instanceId


## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|8466BDFB-C513-4B8D-B4E3-5AB256AB\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|resId|String|ct-cn-4135is2tj194p\*\*\*\*|创建成功的采集器ID。 |

## 示例

请求示例

```
POST /openapi/collectors HTTP/1.1
公共请求头
{
    "dryRun": false, 
    "name": "test_mufei_1", 
    "resType": "fileBeat", 
    "resVersion": "6.8.5_with_community", 
    "vpcId": "vpc-bp12nu14urf0upaf*****", 
    "collectorPaths": [
        "/var/log"
    ], 
    "extendConfigs": [
        {
            "instanceId": "es-cn-nif201ihd0012****", 
            "instanceType": "elasticsearch", 
            "configType": "collectorTargetInstance", 
            "hosts": [
                "es-cn-nif201ihd0012****.elasticsearch.aliyuncs.com:9200"
            ], 
            "userName": "elastic", 
            "password": "*****", 
            "protocol": "HTTP"
        }, 
        {
            "type": "ECSInstanceId", 
            "configType": "collectorDeployMachine", 
            "machines": [
                {
                    "instanceId": "i-bp11u91xgubypcuz****"
                }
            ]
        }
    ], 
    "configs": [
        {
            "fileName": "filebeat.yml", 
            "content": "filebeat.inputs:xxx"
        }, 
        {
            "fileName": "fields.yml", 
            "content": "- key: log\n title: Log file content\n description: >\n Contains log file lines.\n ...."
        }
    ]
}
```

正常返回示例

`XML`格式

```
<Result>
    <resId>ct-cn-4135is2tj194p****</resId>
</Result>
<RequestId>8466BDFB-C513-4B8D-B4E3-5AB256AB****</RequestId>
```

`JSON`格式

```
{
	"Result": {
		"resId": "ct-cn-4135is2tj194p****"
	},
	"RequestId": "8466BDFB-C513-4B8D-B4E3-5AB256AB****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

