# ListEcsInstances

Call the ListEcsInstances to obtain the ECS machine list.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListEcsInstances&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/ecs HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|page|Integer|Query|No|1|The number of returned result pages. |
|size|Integer|Query|No|10|The number of results that each page contains. |
|ecsInstanceIds|String|Query|No|\["i-bp13y63575oypr9d\*\*\*\*","i-bp1gyhphjaj73jsr\*\*\*\*"\]|The ID of ECS instance N. The value can be a JSON array that consists of up to 100 instance IDs. Separate multiple instance IDs with commas \(,\). |
|ecsInstanceName|String|Query|No|test|The ECS instance name. |
|tags|String|Query|No|\[\{ "tagKey":"a","tagValue":"b"\}\]|The ECS instance tag, which must contain:

-   tagKey: tag key
-   tagValue: the tag value |
|vpcId|String|Query|No|vpc-bp16k1dvzxtmagcva\*\*\*\*|The ID of the VPC where the ECS instance is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|11|The number of returned records. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|cloudAssistantStatus|String|true|Cloud Assistant installation status, support:

-   true: Installed
-   false: not installed |
|collectors|Array of collectors| |The list of collector information on the ECS instance. |
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
|hosts|List|\["es-cn-n6w1o1x\*\*\*\*\*.elasticsearch.aliyuncs.com:9200"\]|Collector Output specifies the list of access addresses of the instance. When **configType** For **collectorTargetInstance** When displayed. |
|instanceId|String|es-cn-nif1z89fz003i\*\*\*\*|The instance ID associated with the collector. When **configType** For **collectorTargetInstance** is the instance ID of the collector Output; when **configType** For **collectorDeployMachines** , and **type** For **ACKCluster** is the ACK \(Container Kubernetes\) cluster ID. |
|instanceType|String|elasticsearch|The instance type specified by the collector Output. Support Elasticsearch, logstash. When **configType** For **collectorTargetInstance** When displayed. |
|machines|Array of machines| |The list of ECS machines deployed by the collector. When **configType** For **collectorDeployMachines** , and **type** For **ECSInstanceId** When displayed. |
|agentStatus|String|heartOk or failed|The status of each collector on ECS. Then, you can perform the following operations:

-   heartOk: normal heartbeat
-   heartLost: abnormal heartbeat
-   uninstalled: not installed
-   failed: installation failed |
|instanceId|String|i-bp13y63575oypr9d\*\*\*\*|The list of ECS machine IDs. |
|protocol|String|HTTP|The transmission protocol needs to be consistent with the access protocol of the instance specified by the collector Output. HTTP and HTTPS are supported. When **configType** For **collectorTargetInstance** When displayed. |
|type|String|ECSInstanceId|The type of machine deployed by the collector, when **configType** For **collectorDeployMachine** When displayed. Then, you can perform the following operations:

-   ECSInstanceId:ECS
-   ACKCluster: Container Kubernetes |
|userName|String|elastic|Output specifies the user name of the instance. The default name is elastic. When **configType** For **collectorTargetInstance** or **collectorElasticsearchForKibana** When displayed. |
|gmtCreatedTime|String|2020-06-20T07:26:47.000+0000|The time when the collector was created. |
|gmtUpdateTime|String|2020-06-20T07:26:47.000+0000|The collector update time. |
|name|String|ct-testAbc|The name of the collector. |
|ownerId|String|16852\*\*\*488\*\*\*\*\*|The ID of the Alibaba Cloud account. |
|resId|String|ct-cn-0v3xj86085dvq\*\*\*\*|The collector instance ID. |
|resType|String|fileBeat|The collector type. FileBeat, metricBeat, heartBeat, and auditBeat are supported. |
|resVersion|String|6.8.5\_with\_community|The collector version. When the machine type of the collector is ECS, it is only supported. **6.8.5\_with\_community** . |
|status|String|activing|The collector status. Then, you can perform the following operations:

-   activating: taking effect
-   active: has taken effect |
|vpcId|String|vpc-bp16k1dvzxtm\*\*\*\*\*\*|Virtual Private Cloud ID. where the collector is located |
|ecsInstanceId|String|i-bp14ncqge8wy3l3d\*\*\*\*|The ID of the instance. |
|ecsInstanceName|String|ecsTestName|The ECS instance name. |
|ipAddress|Array of ipAddress| |The IP address information of the ECS instance. |
|host|String|172.16.xx.xx|The IP address. |
|ipType|String|private|The type of the IP address. Then, you can perform the following operations:

-   public: public endpoint
-   private: private address |
|osType|String|linux|The operating system type of the ECS instance. Then, you can perform the following operations:

-   windows:Windows operating system
-   Linux: Linux operating system |
|status|String|running|The status of the ECS instance. Then, you can perform the following operations:

-   running: The cluster is running.
-   starting
-   stopping: Stopping
-   stopped: The nodes are stopped. |
|tags|String|\[ \{ "tagKey": "a", "tagValue": "b" \} \]|The tag information of the ECS instance. |

## Examples

Sample requests

```

     GET /openapi/ecs?page=1&size=10 HTTP/1.1 
   
```

Sample success responses

`JSON`format

```

     { "Result": [ { "ecsInstanceId": "i-bp1gyhphjaj73jsr****", "ecsInstanceName": "test", "status": "running", "ipAddress": [ { "host": "47.98.xx.xx", "ipType": "public" }, { "host": "172.16.xx.xx", "ipType": "private" } ], "tags": [], "collectors": [ { "gmtCreatedTime": "2020-12-30T08:04:32.000+0000", "gmtUpdateTime": "2020-12-30T08:20:48.000+0000", "name": "uptime-test", "resId": "ct-cn-4135is2tj194p****", "resVersion": "6.8.5_with_community", "vpcId": "vpc-bp16k1dvzxtmagcva****", "resType": "heartBeat", "ownerId": "168520994880****", "configs": [ { "fileName": "fields.yml" }, { "fileName": "heartbeat.yml" } ], "status": "active", "extendConfigs": [ { "configType": "collectorTargetInstance", "instanceId": "es-cn-n6w1o1x0w001c****", "instanceType": "elasticsearch", "hosts": [ "es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200" ], "protocol": "HTTP", "userName": "elastic", "enableMonitoring": true }, { "configType": "collectorDeployMachine", "type": "ECSInstanceId", "machines": [ { "instanceId": "i-bp1gyhphjaj73jsr****", "agentStatus": "heartOk" } ], "groupId": "default_ct-cn-4135is2tj194p****" } ], "dryRun": false } ], "osType": "linux", "cloudAssistantStatus": "true" } ], "RequestId": "58E5DE98-33B0-4D9B-B5F6-E70A77C5933E", "Headers": { "X-Total-Count": 2 } } 
   
```

## Error codes

Visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.

