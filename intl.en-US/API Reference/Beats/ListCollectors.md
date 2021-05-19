# ListCollectors

Call ListCollectors to obtain the list information of collectors.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListCollectors&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/collectors HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|resId|String|Query|No|ct-cn-77uqof2s7rg5c\*\*\*\*|The collector ID. |
|name|String|Query|No|collectorName1|The name of the collector. |
|instanceId|String|Query|No|es-cn-nif1q8auz0003\*\*\*\*|The instance ID associated with the collector. |
|page|Integer|Query|No|1|The number of pages of the returned result. Default value: 1, minimum value: 1, maximum value: 200. |
|size|Integer|Query|No|10|The number of results per page. Default value: 20, minimum value: 1, maximum value: 500. |
|sourceType|String|Query|No|ECS|Specify the type of the machine where the collector is deployed, and return all types without filling in. Valid values:

-   ECS:ECS instances
-   ACK: Container Kubernetes cluster |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|5|The number of returned records. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|collectorPaths|List|\["/var/log"\]|The acquisition path of Filebeat. |
|configs|Array of configs| |The configuration file information of the collector. |
|content|String|- key: log\\n title: Log file content\\n description: \>\\n Contains log file lines.\\n ....|The content of the remote file. |
|fileName|String|fields.yml|The name of the mezzanine file. |
|dryRun|Boolean|false|Whether to verify and create a collector. Then, you can perform the following operations:

-   true: only check and not create
-   false: Check and create |
|extendConfigs|Array of extendConfigs| |The extended parameter information. |
|configType|String|collectorDeployMachine|The type of the configuration. Then, you can perform the following operations:

-   collectorTargetInstance: collector Output
-   collectorDeployMachine: Deployment Machine of Collector
-   collector Elasticsearch ForKibana: Elasticsearch instance information that supports Kibana dashboards |
|enableMonitoring|Boolean|true|Whether to enable Monitoring, when **configType** For **collectorTargetInstance** , and **instanceType** For **elasticsearch** When displayed. Then, you can perform the following operations:

-   true
-   false |
|groupId|String|default\_ct-cn-5i2l75bz4776\*\*\*\*|machine group ID. When **configType** For **collectorDeployMachine** When displayed. |
|host|String|es-cn-n6w1o1x0w001c\*\*\*\*-kibana.internal.elasticsearch.aliyuncs.com:5601|The private network access address of Kibana after Kibana Dashboard is enabled. When **configType** For **collectorElasticsearchForKibana** When displayed. |
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|Collector Output specifies the list of access addresses of the instance. When **configType** For **collectorTargetInstance** When displayed. |
|instanceId|String|es-cn-nif1z89fz003i\*\*\*\*|The instance ID associated with the collector. When **configType** For **collectorTargetInstance** is the instance ID of the collector Output; when **configType** For **collectorDeployMachines** , and **type** For **ACKCluster** is the ACK \(Container Kubernetes\) cluster ID. |
|instanceType|String|elasticsearch|The instance type specified by the collector Output. Support Elasticsearch , logstash. When **configType** For **collectorTargetInstance** When displayed. |
|kibanaHost|String|https://es-cn-nif1z89fz003i\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601|The Internet access address of Kibana after Kibana Dashboard is enabled. When **configType** For **collectorElasticsearchForKibana** When displayed. |
|machines|Array of machines| |The list of ECS machines deployed by the collector. When **configType** For **collectorDeployMachines** , and **type** For **ECSInstanceId** When displayed. |
|agentStatus|String|heartOk|The status of each collector on ECS. Then, you can perform the following operations:

-   heartOk: normal heartbeat
-   heartLost: abnormal heartbeat
-   uninstalled: not installed
-   failed: installation failed |
|instanceId|String|i-bp13y63575oypr9d\*\*\*\*|The list of ECS machine IDs. |
|protocol|String|HTTP|The transmission protocol needs to be consistent with the access protocol of the instance specified by the collector Output. HTTP and HTTPS are supported. When **configType** For **collectorTargetInstance** When displayed. |
|successPodsCount|String|8|The number of all successfully collected pod nodes of the ACK cluster. When **configType** For **collectorDeployMachines** , and **type** For **ACKCluster** When displayed. |
|totalPodsCount|String|10|The number of all collected pod nodes of the ACK cluster. When **configType** For **collectorDeployMachines** , and **type** For **ACKCluster** When displayed. |
|type|String|ECSInstanceId|The type of machine deployed by the collector, when **configType** For **collectorDeployMachine** When displayed. Then, you can perform the following operations:

-   ECSInstanceId:ECS
-   ACKCluster: Container Kubernetes |
|userName|String|elastic|Output specifies the user name of the instance. The default name is elastic. When **configType** For **collectorTargetInstance** or **collectorElasticsearchForKibana** When displayed. |
|gmtCreatedTime|String|2020-08-18T02:06:12.000+0000|The time when the collector was created. |
|gmtUpdateTime|String|2020-08-18T09:40:43.000+0000|The collector update time. |
|name|String|FileBeat001|The name of the collector. |
|ownerId|String|168520994880\*\*\*\*|The ID of the Alibaba Cloud account. |
|resId|String|ct-cn-0v3xj86085dvq\*\*\*\*|The collector instance ID. |
|resType|String|fileBeat|The collector type. FileBeat, metricBeat, heartBeat, and auditBeat are supported. |
|resVersion|String|6.8.5\_with\_community|The collector version. |
|status|String|active|The collector status. Then, you can perform the following operations:

-   activating: taking effect
-   active: has taken effect |
|vpcId|String|vpc-bp16k1dvzxtma\*\*\*\*\*|Virtual Private Cloud ID. where the collector is located |

**extendConfigs** There are 3 configType types, namely collectorTargetInstance, collectorElasticsearchForKibana and collectorDeployMachine. Different deployment machines have different returned parameters. The specific combination method is as follows:

-   collectorTargetInstance
    -   ECS

        configType, instanceId, instanceType, hosts, userName, password, protocol, enableMonitoring

    -   ACK

        configType, instanceId, instanceType, userName, password, protocol, enableMonitoring

-   collectorElasticsearchForKibana
    -   ECS

        configType, instanceId, host, kibanaHost, userName, password, protocol

    -   ACK

        configType

-   collectorDeployMachine
    -   ECS

        configType, type, machines, groupId

    -   ACK

        configType, type, instanceId, totalPodsCount, successPodsCount


## Examples

Sample requests

```

     GET /openapi/collectors?resId=ct-cn-77uqof2s7rg5c ****&page=1&size=10&sourceType=ECS HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "gmtCreatedTime": "2020-11-05T12:42:47.000+0000", "gmtUpdateTime": "2020-11-06T06:06:20.000+0000", "name": "fileBeatOnACK", "resId": "ct-cn-6fy17c8z99c7i****", "resVersion": "6.8.5_with_community", "vpcId": "vpc-bp16k1dvzxtmagcva****", "resType": "fileBeat", "ownerId": "168520994880****", "configs": [ { "fileName": "logCollector.yml" }, { "fileName": "Name of index management policy 1" }, { "fileName": "Name of Index Management Policy 2" } ], "status": "active", "extendConfigs": [ { "configType": "collectorTargetInstance", "instanceId": "es-cn-n6w1o1x0w001c****", "instanceType": "elasticsearch", "hosts": [ "es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200" ], "protocol": "HTTP", "userName": "elastic", "enableMonitoring": false }, { "configType": "collectorDeployMachine", "type": "ACKCluster", "instanceId": "c1b9fde5172b84f82b9928e825a7b****" }, { "configType": "collectorElasticsearchForKibana", "instanceId": "es-cn-n6w1o1x0w001c****", "host": "es-cn-n6w1o1x0w001c****-kibana.internal.elasticsearch.aliyuncs.com:5601", "protocol": "HTTPS", "kibanaHost": "https://es-cn-n6w1o1x0w001c****.kibana.elasticsearch.aliyuncs.com:5601", "userName": "elastic" } ], "dryRun": false }, { "gmtCreatedTime": "2020-09-25T10:27:02.000+0000", "gmtUpdateTime": "2020-09-25T10:27:02.000+0000", "name": "fileBeatOnECS", "resId": "ct-cn-6cro0lb0dn66c****", "resVersion": "6.8.5_with_community", "vpcId": "vpc-bp12nu14urf0upaf4****", "resType": "fileBeat", "ownerId": "168520994880****", "collectorPaths": [ "/var/log/" ], "configs": [ { "fileName": "fields.yml" }, { "fileName": "filebeat.yml" } ], "status": "active", "extendConfigs": [ { "configType": "collectorTargetInstance", "instanceId": "ls-cn-v0h1kzca****", "instanceType": "logstash", "hosts": [ "10.7.xx.xx:8007" ], "protocol": "HTTP", "enableMonitoring": false }, { "configType": "collectorDeployMachine", "type": "ECSInstanceId", "machines": [ { "instanceId": "i-bp13y63575oypr9d****", "agentStatus": "heartOk" } ], "groupId": "default_ct-cn-6cro0lb0dn66c****" } ], "dryRun": false } ], "RequestId": "70338AB9-231F-412B-A8C0-239CD32F****", "Headers": { "X-Total-Count": 2 } } 
   
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

