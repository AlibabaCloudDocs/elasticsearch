# DescribeCollector

调用DescribeCollector，获取采集器实例的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeCollector&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/collectors/[ResId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ResId|String|Path|是|ct-cn-rg31ahn82m0qd\*\*\*\*|采集器实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|collectorPaths|List|\["/var/log"\]|Filebeat采集路径。 |
|configs|Array of configs| |采集器的配置文件信息。 |
|content|String|fileBeat.inputs:xxx|文件内容。 |
|fileName|String|filebeat.yml|文件名称。 |
|dryRun|Boolean|false|是否校验并创建采集器。支持：

 -   true：只校验不创建
-   false：校验并创建 |
|extendConfigs|Array of extendConfigs| |采集器扩展配置。 |
|configType|String|collectorDeployMachine|配置类型。支持：

 -   collectorTargetInstance：采集器Output
-   collectorDeployMachine：采集器的部署机器
-   collectorElasticsearchForKibana：支持Kibana仪表盘的Elasticsearch实例信息 |
|enableMonitoring|Boolean|true|是否启用Monitoring，当**configType**为**collectorTargetInstance**时显示。支持：

 -   true：启用
-   false：不启用 |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|机器组ID。当**configType**为**collectorDeployMachine**时显示。 |
|host|String|es-cn-n6w1o1x0w001c\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的私网地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|采集器Output指定实例的访问地址列表。当**configType**为**collectorTargetInstance**时显示。 |
|instanceId|String|es-cn-n6w1o1\*\*\*\*|采集器关联的实例ID。当**configType**为**collectorTargetInstance**时，为采集器Output的实例ID；当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时，为ACK（容器Kubernetes）集群ID。 |
|instanceType|String|elasticsearch|采集器Output指定的实例类型。支持elasticsearch、logstash。当**configType**为**collectorTargetInstance**时显示。 |
|kibanaHost|String|https://es-cn-nif1z89fz003i\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的公网访问地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|machines|Array of machines| |采集器所部署的ECS机器列表信息。当**configType**为**collectorDeployMachines**，且**type**为**ECSInstanceId**时显示。 |
|agentStatus|String|heartOk|ECS上各采集器的状态。支持：

 -   heartOk：心跳正常
-   heartLost：心跳异常
-   uninstalled：未安装
-   failed：安装失败 |
|instanceId|String|i-bp1gyhphjaj73jsr\*\*\*\*|ECS机器ID列表。 |
|protocol|String|HTTP|传输协议，需要与采集器Output指定实例的访问协议保持一致。支持HTTP、HTTPS。当**configType**为**collectorTargetInstance**时显示。 |
|successPodsCount|String|8|ACK集群所有采集成功的Pod节点数。 |
|totalPodsCount|String|10|ACK集群所有采集的Pod节点数。 |
|type|String|ECSInstanceId|采集器部署的机器类型，当**configType**为**collectorDeployMachine**时显示。支持：

 -   ECSInstanceId：ECS
-   ACKCluster：容器Kubernetes |
|userName|String|elastic|采集器Output指定实例的访问用户名，默认为elastic。当**configType**为**collectorTargetInstance**或**collectorElasticsearchForKibana**时显示。 |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|采集器创建时间。 |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|采集器更新时间。 |
|name|String|ct-cn-4135is2tj194p\*\*\*\*|采集器名称。 |
|ownerId|String|16852099488\*\*\*\*\*|账号ID。 |
|resId|String|ct-cn-rg31ahn82m0qd\*\*\*\*|采集器实例ID。 |
|resType|String|fileBeat|采集器类型。支持fileBeat、metricBeat、heartBeat和auditBeat。 |
|resVersion|String|6.8.5\_with\_community|采集器版本。 |
|status|String|active|采集器状态。支持：

 -   activing：生效中
-   active：已生效 |
|vpcId|String|vpc-bp16k1dvzxtma\*\*\*\*\*|采集器所在的专有网络ID。 |

**extendConfigs**中包括3种configType，分别为collectorTargetInstance、collectorElasticsearchForKibana、collectorDeployMachine，部署机器不同，返回的参数不同，具体组合方式如下：

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


## 示例

请求示例

```
GET /openapi/collectors/ct-cn-6z8b5eblvi706**** HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
    "name": "filebeats",
    "resVersion": "6.8.5_with_community",
    "resId": "ct-cn-6fy17c8z99c7i****",
    "resType": "fileBeat",
    "ownerId": "168520994880****",
    "status": "active",
    "vpcId": "vpc-bp16k1dvzxtmagcva****",
    "dryRun": false,
    "gmtCreatedTime": "2020-11-05T12:42:47.000+0000",
    "gmtUpdateTime": "2020-11-06T05:13:10.000+0000",
    "collectorPaths": "/var/log/*.log",
    "configs": {
        "fileName": "/conf/filebeat.yml",
        "content": "fileBeat.inputs:xxx"
    },
    "extendConfigs": [
        {
            "instanceId": "es-cn-n6w1o1x0w001c****",
            "configType": "collectorElasticsearchForKibana",
            "host": "es-cn-n6w1o1x0w001c****-kibana.internal.elasticsearch.aliyuncs.com:5601",
            "kibanaHost": "https://es-cn-n6w1o1x0w001c****.kibana.elasticsearch.aliyuncs.com:5601",
            "userName": "elastic",
            "protocol": "HTTPS"
        },
        {
            "instanceId": "es-cn-n6w1o1x0w001c****",
            "instanceType": "elasticsearch",
            "configType": "collectorTargetInstance",
            "hosts": [
                "es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200",
                "es-cn-nif1z89fz003i****.elasticsearch.aliyuncs.com:9200"
            ],
            "userName": "elastic",
            "protocol": "HTTPS",
            "enableMonitoring": true
        },
        {
            "machines": [
                {
                    "instanceId": "es-cn-n6w1o1x0w001c****",
                    "agentStatus": "heartOk"
                },
                {
                    "instanceId": "es-cn-nif1z89fz003i****",
                    "agentStatus": "heartOk"
                }
            ],
            "type": "ECSInstanceId",
            "configType": "collectorDeployMachine",
            "groupId": "default_ct-cn-6fy17c8z99c7i****"
        }
    ]
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

