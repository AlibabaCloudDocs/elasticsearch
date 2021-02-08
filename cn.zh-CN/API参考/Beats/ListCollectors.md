# ListCollectors

调用ListCollectors，获取采集器列表信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListCollectors&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/collectors HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|resId|String|Query|否|ct-cn-77uqof2s7rg5c\*\*\*\*|采集器ID。 |
|name|String|Query|否|collectorName1|采集器名称。 |
|instanceId|String|Query|否|es-cn-nif1q8auz0003\*\*\*\*|采集器关联的实例ID。 |
|page|Integer|Query|否|1|返回结果的分页数。默认值：1，最小值：1，最大值：200。 |
|size|Integer|Query|否|10|每页的结果数。默认值：20，最小值：1，最大值：500。 |
|sourceType|String|Query|否|ECS|指定采集器部署机器的类型，不填返回全部类型。可选值：

 -   ECS：ECS实例
-   ACK：容器Kubernetes集群 |

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
|host|String|es-cn-n6w1o1x0w001c\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的私网访问地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|采集器Output指定实例的访问地址列表。当**configType**为**collectorTargetInstance**时显示。 |
|instanceId|String|es-cn-nif1z89fz003i\*\*\*\*|采集器关联的实例ID。当**configType**为**collectorTargetInstance**时，为采集器Output的实例ID；当**configType**为**collectorDeployMachines**，且**type**为**ACKCluster**时，为ACK（容器Kubernetes）集群ID。 |
|instanceType|String|elasticsearch|采集器Output指定的实例类型。支持elasticsearch、logstash。当**configType**为**collectorTargetInstance**时显示。 |
|kibanaHost|String|https://es-cn-nif1z89fz003i\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|启用Kibana Dashboard后，Kibana的公网访问地址。当**configType**为**collectorElasticsearchForKibana**时显示。 |
|machines|Array of machines| |采集器所部署的ECS机器列表信息。当**configType**为**collectorDeployMachines**，且**type**为**ECSInstanceId**时显示。 |
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
|gmtCreatedTime|String|2020-08-18T02:06:12.000+0000|采集器创建时间。 |
|gmtUpdateTime|String|2020-08-18T09:40:43.000+0000|采集器更新时间。 |
|name|String|FileBeat001|采集器名称。 |
|ownerId|String|168520994880\*\*\*\*|账号ID。 |
|resId|String|ct-cn-0v3xj86085dvq\*\*\*\*|采集器实例ID。 |
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
GET /openapi/collectors?resId=ct-cn-77uqof2s7rg5c****&page=1&size=10&sourceType=ECS HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <gmtCreatedTime>2020-11-05T12:42:47.000+0000</gmtCreatedTime>
    <gmtUpdateTime>2020-11-06T06:06:20.000+0000</gmtUpdateTime>
    <name>fileBeatOnACK</name>
    <resId>ct-cn-6fy17c8z99c7i****</resId>
    <resVersion>6.8.5_with_community</resVersion>
    <vpcId>vpc-bp16k1dvzxtmagcva****</vpcId>
    <resType>fileBeat</resType>
    <ownerId>168520994880****</ownerId>
    <configs>
        <fileName>logCollector.yml</fileName>
    </configs>
    <configs>
        <fileName>索引管理策略1的名称</fileName>
    </configs>
    <configs>
        <fileName>索引管理策略2的名称</fileName>
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
        <configType>collectorDeployMachine</configType>
        <type>ACKCluster</type>
        <instanceId>c1b9fde5172b84f82b9928e825a7b****</instanceId>
    </extendConfigs>
    <extendConfigs>
        <configType>collectorElasticsearchForKibana</configType>
        <instanceId>es-cn-n6w1o1x0w001c****</instanceId>
        <host>es-cn-n6w1o1x0w001c****-kibana.internal.elasticsearch.aliyuncs.com:5601</host>
        <protocol>HTTPS</protocol>
        <kibanaHost>https://es-cn-n6w1o1x0w001c****.kibana.elasticsearch.aliyuncs.com:5601</kibanaHost>
        <userName>elastic</userName>
    </extendConfigs>
    <dryRun>false</dryRun>
</Result>
<Result>
    <gmtCreatedTime>2020-09-25T10:27:02.000+0000</gmtCreatedTime>
    <gmtUpdateTime>2020-09-25T10:27:02.000+0000</gmtUpdateTime>
    <name>fileBeatOnECS</name>
    <resId>ct-cn-6cro0lb0dn66c****</resId>
    <resVersion>6.8.5_with_community</resVersion>
    <vpcId>vpc-bp12nu14urf0upaf4****</vpcId>
    <resType>fileBeat</resType>
    <ownerId>168520994880****</ownerId>
    <collectorPaths>/var/log/</collectorPaths>
    <configs>
        <fileName>fields.yml</fileName>
    </configs>
    <configs>
        <fileName>filebeat.yml</fileName>
    </configs>
    <status>active</status>
    <extendConfigs>
        <configType>collectorTargetInstance</configType>
        <instanceId>ls-cn-v0h1kzca****</instanceId>
        <instanceType>logstash</instanceType>
        <hosts>10.7.xx.xx:8007</hosts>
        <protocol>HTTP</protocol>
        <enableMonitoring>false</enableMonitoring>
    </extendConfigs>
    <extendConfigs>
        <configType>collectorDeployMachine</configType>
        <type>ECSInstanceId</type>
        <machines>
            <instanceId>i-bp13y63575oypr9d****</instanceId>
            <agentStatus>heartOk</agentStatus>
        </machines>
        <groupId>default_ct-cn-6cro0lb0dn66c****</groupId>
    </extendConfigs>
    <dryRun>false</dryRun>
</Result>
<RequestId>70338AB9-231F-412B-A8C0-239CD32F****</RequestId>
<Headers>
    <X-Total-Count>2</X-Total-Count>
</Headers>
```

`JSON`格式

```
{
  "Result": [
    {
      "gmtCreatedTime": "2020-11-05T12:42:47.000+0000",
      "gmtUpdateTime": "2020-11-06T06:06:20.000+0000",
      "name": "fileBeatOnACK",
      "resId": "ct-cn-6fy17c8z99c7i****",
      "resVersion": "6.8.5_with_community",
      "vpcId": "vpc-bp16k1dvzxtmagcva****",
      "resType": "fileBeat",
      "ownerId": "168520994880****",
      "configs": [
        {
          "fileName": "logCollector.yml"
        },
        {
          "fileName": "索引管理策略1的名称"
        },
        {
          "fileName": "索引管理策略2的名称"
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
          "configType": "collectorDeployMachine",
          "type": "ACKCluster",
          "instanceId": "c1b9fde5172b84f82b9928e825a7b****"
        },
        {
          "configType": "collectorElasticsearchForKibana",
          "instanceId": "es-cn-n6w1o1x0w001c****",
          "host": "es-cn-n6w1o1x0w001c****-kibana.internal.elasticsearch.aliyuncs.com:5601",
          "protocol": "HTTPS",
          "kibanaHost": "https://es-cn-n6w1o1x0w001c****.kibana.elasticsearch.aliyuncs.com:5601",
          "userName": "elastic"
        }
      ],
      "dryRun": false
    },
    {
      "gmtCreatedTime": "2020-09-25T10:27:02.000+0000",
      "gmtUpdateTime": "2020-09-25T10:27:02.000+0000",
      "name": "fileBeatOnECS",
      "resId": "ct-cn-6cro0lb0dn66c****",
      "resVersion": "6.8.5_with_community",
      "vpcId": "vpc-bp12nu14urf0upaf4****",
      "resType": "fileBeat",
      "ownerId": "168520994880****",
      "collectorPaths": [
        "/var/log/"
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
          "instanceId": "ls-cn-v0h1kzca****",
          "instanceType": "logstash",
          "hosts": [
            "10.7.xx.xx:8007"
          ],
          "protocol": "HTTP",
          "enableMonitoring": false
        },
        {
          "configType": "collectorDeployMachine",
          "type": "ECSInstanceId",
          "machines": [
            {
              "instanceId": "i-bp13y63575oypr9d****",
              "agentStatus": "heartOk"
            }
          ],
          "groupId": "default_ct-cn-6cro0lb0dn66c****"
        }
      ],
      "dryRun": false
    }
  ],
  "RequestId": "70338AB9-231F-412B-A8C0-239CD32F****",
  "Headers": {
    "X-Total-Count": 2
  }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

