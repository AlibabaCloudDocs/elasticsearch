# UpdateCollector

调用UpdateCollector，更新采集器实例信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateCollector&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PUT /openapi/collectors/[ResId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ResId|String|Path|是|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器实例ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定待更新采集器的配置信息。

|参数

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|dryRun

|boolean

|是

|false

|是否校验并更新采集器。可选值：true（只校验不更新）、false（校验并更新）。 |
|name

|String

|是

|ct-test

|采集器名称。 |
|status

|String

|是

|activing

|采集器状态。可选值：activing（生效中）、active（已生效）。 |
|resType

|String

|是

|fileBeat

|采集器类型。可选值：fileBeat、metricBeat、heartBeat、auditBeat。 |
|vpcId

|String

|是

|vpc-bp16k1dvzxtma\*\*\*\*\*

|采集器所在的专有网络ID。 |
|resVersion

|String

|是

|6.8.5\_with\_community

|采集器版本。 |
|ownerId

|String

|是

|16852099488\*\*\*\*\*

|账号ID。 |
|gmtCreatedTime

|Date

|是

|2020-06-20T07:26:47.000+0000

|采集器创建时间。 |
|gmtUpdateTime

|Date

|是

|2020-06-20T07:26:47.000+0000

|采集器更新时间。 |
|collectorPaths

|List<String\>

|否

|\["/var/log"\]

|Filebeat采集路径。 |
|configs

|List

|是

| |采集器的配置文件信息。 |
|└fileName

|String

|是

|filebeat.yml

|文件名称。 |
|└content

|String

|是

|"filebeat.inputs:xxx"

|文件内容。 |
|extendConfigs

|Array

|是

| |采集器扩展配置。 |
|└configType

|String

|是

|collectorElasticsearchForKibana

|配置类型。可选值：collectorTargetInstance（采集器Output）、collectorDeployMachine（采集器的部署机器）、collectorElasticsearchForKibana（支持Kibana仪表盘的Elasticsearch实例信息）。 |
|└type

|String

|是

|ECSInstanceId

|采集器部署的机器类型。可选值：ECSInstanceId（ECS）、ACKCluster（容器Kubernetes）。当**configType**为**collectorDeployMachine**时必填。 |
|└machines

|Array

|否

| |采集器所部署的ECS机器列表信息。当**configType**为**collectorDeployMachines**，且**type**为**ECSInstanceId**时，必填。 |
|└└instanceId

|String

|否

|i-bp13y63575oypr9d\*\*\*\*

|ECS机器ID列表。 |
|└└agentStatus

|String

|否

|failed

|ECS上各采集器的状态。可选值：heartOk（心跳正常）、heartLost（心跳异常）、uninstalled（未安装）、failed（安装失败）。 |
|└groupId

|String

| |default\_ct-cn-5i2l75bz4776\*\*\*\*

|机器组ID。当**configType**为**collectorDeployMachine**时，必填。 |
|└instanceId

|String

|是

|es-cn-nif1z89fz003i\*\*\*\*

|采集器关联的实例ID。当**configType**为**collectorTargetInstance**时，为采集器Output的实例ID；当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时，为ACK集群ID。 |
|└protocol

|String

|是

|HTTP

|传输协议，需要与采集器Output指定实例的访问协议保持一致。可选值HTTP、HTTPS。当**configType**为**collectorTargetInstance**时必填。 |
|└userName

|String

|是

|elastic

|采集器Output指定实例的访问用户名，默认为elastic。当**configType**为**collectorTargetInstance**或**collectorElasticsearchForKibana**时必填。 |
|└enableMonitoring

|Boolean

|是

|true

|是否启用Monitoring，当**configType**为**collectorTargetInstance**，且**instanceType**为**elasticsearch**时必填。可选值：true（启用）、false（不启用）。 |
|└hosts

|List<String\>

|否

|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]

|采集器Output指定实例的访问地址列表。当**configType**为**collectorTargetInstance**时必填。 |
|└instanceType

|String

|是

|elasticsearch

|采集器Output指定的实例类型。可选值：elasticsearch、logstash。当**configType**为**collectorTargetInstance**时必填。 |
|└host

|String

|否

|es-cn-n6w1o1x0w001c\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601

|启用Kibana Dashboard后，Kibana的私网访问地址。当**configType**为**collectorElasticsearchForKibana**时必填。 |
|└kibanaHost

|String

|否

|https://es-cn-nif1z89fz003i\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601

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

        configType、type、instanceId、totalPodsCount、successPodsCount


## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|collectorPaths|List|\["/var/log"\]|Filebeat的采集路径。 |
|configs|Array of configs| |采集器的配置文件信息。 |
|content|String|filebeat.inputs:xxx|文件内容 |
|fileName|String|filebeat.yml|文件名称 |
|dryRun|Boolean|false|是否校验并创建采集器。支持：

 -   true：只校验不创建
-   false：校验并创建 |
|extendConfigs|Array of extendConfigs| |扩展参数信息。 |
|configType|String|collectorDeployMachine|配置类型。支持：

 -   collectorTargetInstance：采集器Output
-   collectorDeployMachine：采集器的部署机器
-   collectorElasticsearchForKibana：支持Kibana仪表盘的Elasticsearch实例信息 |
|enableMonitoring|Boolean|true|是否启用Monitoring，当**configType**为**collectorTargetInstance**，且**instanceType**为**elasticsearch**时显示。支持true（启用）、false（不启用）。 |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|机器组ID。当**configType**为**collectorDeployMachine**时显示。 |
|host|String|es-cn-n6w1o1x0w001c\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的私网访问地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|采集器Output指定实例的访问地址列表。当**configType**为**collectorTargetInstance**时显示。 |
|instanceId|String|es-cn-nif1z89fz003i\*\*\*\*|采集器关联的实例ID。当**configType**为**collectorTargetInstance**时，为采集器Output的实例ID；当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时，为ACK（容器Kubernetes）集群ID。 |
|instanceType|String|elasticsearch|采集器Output指定的实例类型。支持elasticsearch、logstash。当**configType**为**collectorTargetInstance**时显示。 |
|kibanaHost|String|https://es-cn-nif1z89fz003i\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的公网访问地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|machines|Array of machines| |collectorDeployMachine类型专用：

 部署ECS机器/ACK集群信息 |
|agentStatus|String|heartOk|ECS上各采集器的状态。支持：

 -   heartOk：心跳正常
-   heartLost：心跳异常
-   uninstalled：未安装
-   failed：安装失败 |
|instanceId|String|i-bp13y63575oypr9d\*\*\*\*|ECS机器ID列表。 |
|protocol|String|HTTP|传输协议，需要与采集器Output指定实例的访问协议保持一致。支持HTTP、HTTPS。当**configType**为**collectorTargetInstance**时显示。 |
|successPodsCount|String|8|ACK集群所有成功采集的Pod节点数。当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时显示。 |
|totalPodsCount|String|10|ACK集群所有采集的Pod节点数。当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时显示。 |
|type|String|ECSInstanceId|采集器部署的机器类型，当**configType**为**collectorDeployMachine**时显示。支持：

 -   ECSInstanceId：ECS
-   ACKCluster：容器Kubernetes |
|userName|String|elastic|采集器Output指定实例的访问用户名，默认为elastic。当**configType**为**collectorTargetInstance**或**collectorElasticsearchForKibana**时显示。 |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|采集器创建时间。 |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|采集器更新时间。 |
|name|String|ct-test|采集器名称。 |
|ownerId|String|16852099488\*\*\*\*\*|账号ID。 |
|resId|String|ct-cn-0v3xj86085dvq\*\*\*\*|采集器实例ID。 |
|resType|String|fileBeat|采集器类型。支持fileBeat、metricBeat、heartBeat和auditBeat。 |
|resVersion|String|6.8.5\_with\_community|采集器版本。 |
|status|String|active|采集器状态。支持：

 -   activing：生效中
-   active：已生效 |
|vpcId|String|vpc-bp16k1dvzxtma\*\*\*\*\*|采集器所在的专有网络ID。 |

## 示例

请求示例

```
PUT /openapi/collectors/ct-cn-77uqof2s7rg5c**** HTTP/1.1
公共请求头

{
    "dryRun": false, 
    "name": "fileBeat", 
    "resVersion": "6.8.5_with_community", 
    "extendConfigs": [
        {
            "instanceId": "es-cn-sfd", 
            "configType": "collectorElasticsearchForKibana", 
            "hosts": [
                "es-cn-abc.elasticsearch.aliyuncs.com:9200", 
                "es-cn-abd.elasticsearch.aliyuncs.com:9200"
            ], 
            "userName": "elastic", 
            "password": "******", 
            "protocol": "https"
        }, 
        {
            "instanceId": "ec-cn-targ", 
            "instanceType": "elasticsearch", 
            "configType": "collectorTargetInstance", 
            "hosts": [
                "es-cn-abc.elasticsearch.aliyuncs.com:9200", 
                "es-cn-abd.elasticsearch.aliyuncs.com:9200"
            ], 
            "userName": "elastic", 
            "password": "******", 
            "protocol": "https"
        }, 
        {
            "values": [
                {
                    "instanceId": "id1"
                }, 
                {
                    "instanceId": "id2"
                }
            ], 
            "type": "ECSInstanceId", 
            "configType": "collectorDeployMachine"
        }
    ], 
    "resType": "fileBeat", 
    "vpcId": "vpc-cn-abc", 
    "configs": [
        {
            "fileName": "filebeat.yml", 
            "content": "filebeat.inputs:xxx"
        }
    ]
}
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D***",
    "Result": {
        "configs": {
            "fileName": "filebeat.yml",
            "content": "filebeat.inputs:xxx"
        },
        "dryRun": false,
        "resType": "fileBeat",
        "ownerId": "16852099488*****",
        "resId": "ct-cn-0v3xj86085dvq****",
        "collectorPaths": ["/var/log"],
        "gmtUpdateTime": "2020-06-20T07:26:47.000+0000",
        "extendConfigs": [
            {
                "enableMonitoring": true,
                "groupId": "default_ct-cn-5i2l75bz4776****",
                "instanceType": "elasticsearch",
                "type": "ECSInstanceId",
                "userName": "elastic",
                "configType": "collectorDeployMachine",
                "protocol": "HTTP",
                "instanceId": "es-cn-nif1z89fz003i****",
                "host": "es-cn-n6w1o1x0w001c****-kibana.internal.elasticsearch.aliyuncs.com:5601",
                "kibanaHost": "https://es-cn-nif1z89fz003i****.kibana.elasticsearch.aliyuncs.com:5601",
                "totalPodsCount": 10,
                "successPodsCount": 8
            },
            {
                "machines": {
                    "agentStatus": "heartOk",
                    "instanceId": "i-bp13y63575oypr9d****"
                }
            },
            {
                "hosts": ["es-cn-n6w1o1x*****.elasticsearch.aliyuncs.com:9200"]
            }
        ],
        "resVersion": "6.8.5_with_community",
        "vpcId": "vpc-bp16k1dvzxtma*****",
        "name": "ct-test",
        "gmtCreatedTime": "2020-06-20T07:26:47.000+0000",
        "status": "active"
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

