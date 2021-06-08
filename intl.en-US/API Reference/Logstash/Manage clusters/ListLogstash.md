# ListLogstash

Queries the detailed information of all Logstash clusters or a specified Logstash cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListLogstash&type=ROA&version=2017-06-13)

## Request headers

/openapi/logstashes

## Request syntax

```

     GET /openapi/logstashes HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|page|Integer|Query|No|1|The page number of the returned page. Default value: 1. |
|size|Integer|Query|No|10|The number of entries to return on each page. Default value: 20. |
|description|String|Query|No|ls-cn-abc|The name of the instance. You can specify a keyword to match multiple instances. for example, if an instance named abc is queried, all instances named abc, abcde, xyabc, or xabcy may be returned. |
|instanceId|String|Query|No|ls-cn-n6w1o5jq\*\*\*\*|The ID of the instance. |
|version|String|Query|No|5.5.3\_with\_X-Pack|The version of the instance. |
|resourceGroupId|String|Query|No|rg-acfm2h5vbzd\*\*\*\*|The ID of the resource group. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |the request header information. |
|X-Total-Count|Integer|10|The number of instances that match the query string. |
|RequestId|String|AC442F2F-5068-4434-AA21-E78947A9\*\*\*\*|The ID of the request. |
|Result|Array of Instance| |Detailed information about the matching instances. |
|Tags|Array of tags| |The tags of the ALB instance. |
|TagKey|String|env|The tag key. |
|TagValue|String|dev|The value of the tag to be modified. |
|createdAt|String|2018-07-13T03:58:07.253Z|The time when the instance was created. |
|description|String|ls-cn-abc|The name of the instance. |
|instanceId|String|ls-cn-n6w1o5jq\*\*\*\*|The ID of the instance. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|Network type, currently only supports Virtual Private Cloud \(VPC\). |
|vpcId|String|vpc-abc|The ID of the virtual private cloud \(VPC\). |
|vsArea|String|cn-hangzhou-\*|The zone where the instance is deployed. |
|vswitchId|String|vsw-def|The ID of the vSwitch. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration information of the data node. |
|disk|Integer|50|The node disk size. |
|diskEncryption|Boolean|false|Whether to use disk encryption:

-   true
-   false |
|diskType|String|cloud\_ssd|The type of the disk. |
|spec|String|logstash.n4.small|The instance type. |
|paymentType|String|postpaid|The billing mode of the instance. Support: prepaid \(package year and month\) and postpaid \(pay-as-you-go\). |
|status|String|active|The state of the instance. Four states are supported: active, active, inactive, and invalid. |
|updatedAt|String|2018-07-18T10:10:04.484Z|The time when the instance was last updated. |
|version|String|6.7.0\_with\_X-Pack|The version of the instance. Currently, only 6.7.0\_with\_X-Pack and 7.4.0\_with\_X-Pack are supported. |

The following parameters are also included in the returned data.

|Parameter

|Type

|Example

|Description |
|-----------|------|---------|-------------|
|enablePublic

|Boolean

|false

|whether to enable public access. the default value is false. |
|commodityCode

|String

|elasticsearch\_logstash\_post

|The service code. |
|endTime

|Long

|4749897600000

|Package year-to-month instance, the last expiration time. |
|clusterTasks

|Array

|\[\]

|The list of tasks for the instance. |
|resourceGroupId

|String

|rg-acfm2h5vbzd\*\*\*\*

|The ID of the resource group to which the VPC belongs. |
|zoneCount

|Integer

|1

|The number of zones. |
|protocol

|String

|HTTP

|The access protocol of the instance. |
|zoneInfos

|Array

| |Zone information. |
|└zoneId

|String

|cn-hangzhou-i

|The ID of the zone. |
|└status

|String

|NORMAL

|The status of the zone. Support: **ISOLATION** \(offline\) and **NORMAL** \(normal\). |
|instanceType

|String

|logstash

|The engine type of the instance. |
|inited

|Boolean

|true

|Whether the instance has completed initialization. |
|config

|Array

|\[\]

|Configure ECS instances. |
|endpointList

|Array

| |The node information. |
|└host

|String

|172.16.xx.xx

|The IP address of the node. |
|└port

|Integer

|9200

|The access port number of the node. |
|└zoneId

|String

|cn-hangzhou-i

|The zone ID where the node is located. |

**Note:** └ indicates a child parameter.

## Examples

Sample requests

```

     GET /openapi/logstashes?description=abc&page=1&size=10 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "instanceId": "ls-cn-n6w1o5jq****", "version": "6.7.0_with_X-Pack", "description": "test", "nodeAmount": 1, "paymentType": "postpaid", "status": "active", "enablePublic": false, "nodeSpec": { "spec": "elasticsearch.sn1ne.large", "disk": 20, "diskType": "cloud_ssd" }, "networkConfig": { "vpcId": "vpc-bp16k1dvzxtmagcva****", "vswitchId": "vsw-bp1k4ec6s7sjdbudw****", "vsArea": "cn-hangzhou-i", "type": "vpc" }, "createdAt": "2020-05-27T01:30:15.947Z", "updatedAt": "2020-05-27T01:40:51.333Z", "commodityCode": "elasticsearch_logstash_post", "extendConfigs": [], "endTime": 4746268800000, "clusterTasks": [], "resourceGroupId": "rg-acfm2h5vbzd****", "zoneCount": 1, "protocol": "HTTP", "zoneInfos": [ { "zoneId": "cn-hangzhou-i", "status": "NORMAL" } ], "instanceType": "logstash", "inited": true, "tags": [], "config": {}, "endpointList": [ { "host": "172.16.**.**", "port": 9600, "zoneId": "cn-hangzhou-i" } ] } ], "RequestId": "918C05D8-4689-4A79-B6D5-D2500991****", "Headers": { "X-Total-Count": 1 } } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

