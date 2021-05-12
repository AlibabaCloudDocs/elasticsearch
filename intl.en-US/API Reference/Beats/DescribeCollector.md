# DescribeCollector

call DescribeCollector get collector instance details of

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeCollector&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/collectors/[ResId] HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ResId|String|Path|Yes|ct-cn-rg31ahn82m0qd\*\*\*\*|The collector instance ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The returned results. |
|collectorPaths|List|\["/var/log"\]|Filebeat collection path. |
|configs|Array of configs| |The configuration file information of the collector. |
|content|String|fileBeat.inputs:xxx|The content of the remote file. |
|fileName|String|filebeat.yml|The name of the mezzanine file. |
|dryRun|Boolean|false|Whether to verify and create a collector. Then, you can perform the following operations:

-   true: only check and not create
-   false: Check and create |
|extendConfigs|Array of extendConfigs| |Collector expansion configuration. |
|configType|String|collectorDeployMachine|The type of the configuration. Then, you can perform the following operations:

-   collectorTargetInstance: collector Output
-   collectorDeployMachine: Deployment Machine of Collector
-   collector Elasticsearch ForKibana: Elasticsearch instance information that supports Kibana dashboards |
|enableMonitoring|Boolean|true|Whether to enable Monitoring, when **configType** For **collectorTargetInstance** When displayed. Then, you can perform the following operations:

-   true
-   false |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|machine group ID. When **configType** For **collectorDeployMachine** When displayed. |
|host|String|es-cn-n6w1o1x0w001c\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601|The private network address of Kibana after Kibana Dashboard is enabled. When **configType** For **collectorElasticsearchForKibana** When displayed. |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|Collector Output specifies the list of access addresses of the instance. When **configType** For **collectorTargetInstance** When displayed. |
|instanceId|String|es-cn-n6w1o1\*\*\*\*|The instance ID associated with the collector. When **configType** For **collectorTargetInstance** is the instance ID of the collector Output; when **configType** For **collectorDeployMachines** , and **type** For **ACKCluster** is the ACK \(Container Kubernetes\) cluster ID. |
|instanceType|String|elasticsearch|The instance type specified by the collector Output. Support Elasticsearch 、logstash. When **configType** For **collectorTargetInstance** When displayed. |
|kibanaHost|String|https://es-cn-nif1z89fz003i\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|The Internet access address of Kibana after Kibana Dashboard is enabled. When **configType** For **collectorElasticsearchForKibana** When displayed. |
|machines|Array of machines| |The list of ECS machines deployed by the collector. When **configType** For **collectorDeployMachines** , and **type** For **ECSInstanceId** When displayed. |
|agentStatus|String|heartOk|The status of each collector on ECS. Then, you can perform the following operations:

-   heartOk: normal heartbeat
-   heartLost: abnormal heartbeat
-   uninstalled: not installed
-   failed: installation failed |
|instanceId|String|i-bp1gyhphjaj73jsr\*\*\*\*|The list of ECS machine IDs. |
|protocol|String|HTTP|The transmission protocol needs to be consistent with the access protocol of the instance specified by the collector Output. HTTP and HTTPS are supported. When **configType** For **collectorTargetInstance** When displayed. |
|successPodsCount|String|8|The number of pod nodes that are successfully collected in the ACK cluster. |
|totalPodsCount|String|10|The number of all collected pod nodes of the ACK cluster. |
|type|String|ECSInstanceId|The type of machine deployed by the collector, when **configType** For **collectorDeployMachine** When displayed. Then, you can perform the following operations:

-   ECSInstanceId:ECS
-   ACKCluster: Container Kubernetes |
|userName|String|elastic|Output specifies the user name of the instance. The default name is elastic. When **configType** For **collectorTargetInstance** or **collectorElasticsearchForKibana** When displayed. |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|The time when the collector was created. |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|The collector update time. |
|name|String|ct-cn-4135is2tj194p\*\*\*\*|The name of the collector. |
|ownerId|String|16852099488\*\*\*\*\*|The ID of the Alibaba Cloud account. |
|resId|String|ct-cn-rg31ahn82m0qd\*\*\*\*|The collector instance ID. |
|resType|String|fileBeat|The collector type. FileBeat, metricBeat, heartBeat, and auditBeat are supported. |
|resVersion|String|6.8.5\_with\_community|The collector version. |
|status|String|active|The collector status. Then, you can perform the following operations:

-   activating: taking effect
-   active: has taken effect |
|vpcId|String|vpc-bp16k1dvzxtma\*\*\*\*\*|Virtual Private Cloud ID. where the collector is located |

**extendConfigs** There are 3 configType types, namely collectorTargetInstance, collectorElasticsearchForKibana and collectorDeployMachine. Different deployment machines have different returned parameters. The specific combination method is as follows:

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


## Examples

Sample requests

```

     GET /openapi/collectors/ct-cn-6z8b5eblvi706**** HTTP/1.1 Public Request Header 
   
```

Sample success responses

`JSON` format

```

     { "name": "filebeats", "resVersion": "6.8.5_with_community", "resId": "ct-cn-6fy17c8z99c7i****", "resType": "fileBeat", "ownerId": "168520994880****", "status": "active", "vpcId": "vpc-bp16k1dvzxtmagcva****", "dryRun": false, "gmtCreatedTime": "2020-11-05T12:42:47.000+0000", "gmtUpdateTime": "2020-11-06T05:13:10.000+0000", "collectorPaths": "/var/log/*.log", "configs": { "fileName": "/conf/filebeat.yml", "content": "fileBeat.inputs:xxx" }, "extendConfigs": [ { "instanceId": "es-cn-n6w1o1x0w001c****", "configType": "collectorElasticsearchForKibana", "host": "es-cn-n6w1o1x0w001c****-kibana.internal.elasticsearch.aliyuncs.com:5601", "kibanaHost": "https://es-cn-n6w1o1x0w001c****.kibana.elasticsearch.aliyuncs.com:5601", "userName": "elastic", "protocol": "HTTPS" }, { "instanceId": "es-cn-n6w1o1x0w001c****", "instanceType": "elasticsearch", "configType": "collectorTargetInstance", "hosts": [ "es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200", "es-cn-nif1z89fz003i****.elasticsearch.aliyuncs.com:9200" ], "userName": "elastic", "protocol": "HTTPS", "enableMonitoring": true }, { "machines": [ { "instanceId": "es-cn-n6w1o1x0w001c****", "agentStatus": "heartOk" }, { "instanceId": "es-cn-nif1z89fz003i****", "agentStatus": "heartOk" } ], "type": "ECSInstanceId", "configType": "collectorDeployMachine", "groupId": "default_ct-cn-6fy17c8z99c7i****" } ] } 
   
```

## Error codes

Visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.

