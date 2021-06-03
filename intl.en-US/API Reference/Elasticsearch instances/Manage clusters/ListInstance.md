# ListInstance

Call ListInstance to display the details of all instances in the list.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListInstance&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/instances HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|page|Integer|Query|No|1|The page number of the returned page.

Start value: **1** , Default value: **1** . |
|size|Integer|Query|No|10|The number of entries per page set when you query by page. Default value: **20** . |
|description|String|Query|No|es-cn-abc|The name of the instance, which supports fuzzy query. For example, search **abc** of all instances, it may return **abc** , **abcde** , **xyabc** , **xabcy** All instances of. |
|instanceId|String|Query|No|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|esVersion|String|Query|No|5.3\_with\_X-Pack|The version of the instance. |
|resourceGroupId|String|Query|No|rg-aekzvowej3i\*\*\*\*|The ID of the resource group to which the VPC belongs. |
|tags|String|Query|No|dev-env|Details about the tags. |
|vpcId|String|Query|No|vpc-bp16k1dvzxtmagcva\*\*\*\*|Virtual Private Cloud ID. where the instance is located |
|zoneId|String|Query|No|cn-hangzhou-i|The zone ID of the instance. |
|paymentType|String|Query|No|postpaid|The billing method of the instance. Optional values: postpaid \(pay-as-you-go\) and prepaid \(package year and month\). |
|instanceCategory|String|Query|No|advanced|The version of the instance. Optional values: x-pack \(Commercial Edition\), advanced \(Enhanced Edition\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|10|The number of instances that match the query string. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Instance| |The return results. |
|advancedDedicateMaster|Boolean|false|Indicates whether the instance contains dedicated master nodes. The meaning of the value is as follows:

-   true: The resource tags are returned.
-   flase: does not contain. |
|clientNodeConfiguration|Struct| |Coordinate node configurations. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|20|The size of the node storage space, in GB. |
|diskType|String|cloud\_efficiency|The storage type of the node. Only high-efficiency cloud disks \(cloud\_efficiency\) are supported. |
|spec|String|elasticsearch.sn2ne.large|The node specification. |
|createdAt|String|2018-07-13T03:58:07.253Z|The time when the instance was created. |
|dedicateMaster|Boolean|false|Whether to include exclusive master nodes \(old version\), the value meaning is as follows:

-   true: The resource tags are returned.
-   flase: does not contain. |
|description|String|es-cn-abc|The name of the ApsaraDB for Redis instance. |
|elasticDataNodeConfiguration|Struct| |Elastic data node configurations. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|20|The size of the node storage space, in GB. |
|diskEncryption|Boolean|true|Whether to enable cloud disk encryption for the node, the value meaning is as follows:

-   true: The instant access feature was enabled.
-   flase: not on. |
|diskType|String|cloud\_ssd|The node storage type. Support: cloud\_ssd\(SSD cloud disk\), cloud\_essd \(Enhanced SSD\), cloud\_efficiency \(efficient cloud disk\). |
|spec|String|elasticsearch.sn2ne.large|The node specification. |
|esVersion|String|5.5.3\_with\_X-Pack|The version of the instance. |
|extendConfigs|List|\[\{ "configType": "aliVersion", "aliVersion": "ali1.3.0" \}\]|Cluster scaling parameter configurations. |
|instanceId|String|es-cn-abc|The ID of the Elasticsearch instance. |
|kibanaConfiguration|Struct| |The configuration of Kibana nodes. |
|amount|Integer|1|The number of DRDS server nodes. |
|disk|Integer|20|The size of the node storage space, in GB. |
|diskType|String|cloud\_ssd|The node storage type. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|masterConfiguration|Struct| |The configuration of dedicated master nodes. |
|amount|Integer|3|The number of nodes. |
|disk|Integer|20|The size of the node storage space, in GB. |
|diskType|String|cloud\_ssd|The node storage type. Only cloud\_ssd\(SSD cloud disk\) is supported. |
|spec|String|elasticsearch.sn2ne.large|The node specification. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|Network type, only Virtual Private Cloud \(VPC\) is supported. |
|vpcId|String|vpc-abc|The ID of the VPC. |
|vsArea|String|cn-hangzhou-e|The zone where the instance is deployed. |
|vswitchId|String|vsw-def|The ID of the vSwitch. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration of data nodes. |
|disk|Integer|50|The size of the storage space of the node, in GB. |
|diskEncryption|Boolean|false|Whether to use disk encryption:

-   true
-   false |
|diskType|String|cloud\_ssd|The storage type of the node. Support: cloud\_ssd\(SSD cloud disk\) and cloud\_efficiency \(efficient cloud disk\). |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|paymentType|String|postpaid|The billing method of the ECS instance.

Support: **prepaid** \(package year and month\) and **postpaid** \(pay-as-you-go\). |
|postpaidServiceStatus|String|active|The postpaid service status of the overlay of a prepaid instance. Valid values: **active** \(normal\), **closed** \(closed\), **indebt** \(arrears frozen\). |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|The ID of the resource group. |
|status|String|active|The state of the instance.

Support: **active** \(normal\), **activating** \(in effect\), **inactive** \(frozen\) and **invalid** \(Failure\). |
|tags|Array of Tag| |Details about the tags. |
|tagKey|String|env|The tag key of the ENI. |
|tagValue|String|dev|The tag value of the ENI. |
|updatedAt|String|2018-07-18T10:10:04.484Z|The time when the instance was last updated. |

**Note:** In the following return example, this article only guarantees that the parameters in the return data list are included, and the parameters not mentioned are for reference only. The program cannot force to rely on obtaining these parameters.

## Examples

Sample requests

```

     GET /openapi/instances?description=abc&page=1&size=10 
   
```

Sample success responses

`JSON` format

```

     { "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1****", "Headers": { "X-Total-Count": 10 }, "Result": { "createdAt": "2018-07-13T03:58:07.253Z", "instanceId": "es-cn-abc", "description": "es-cn-abc", "resourceGroupId": "rg-aekzvowej3i****", "dedicateMaster": false, "nodeAmount": 2, "esVersion": "5.5.3_with_X-Pack", "advancedDedicateMaster": false, "postpaidServiceStatus": "active", "updatedAt": "2018-07-18T10:10:04.484Z", "status": "active", "paymentType": "postpaid", "tags": { "tagValue": "dev", "tagKey": "env" }, "extendConfigs": "[{ \"configType\": \"aliVersion\",\t\"aliVersion\": \"ali1.3.0\" }]", "clientNodeConfiguration": { "disk": 20, "amount": 3, "diskType": "cloud_efficiency", "spec": "elasticsearch.sn2ne.large" }, "elasticDataNodeConfiguration": { "disk": 20, "amount": 3, "diskType": "cloud_ssd", "diskEncryption": true, "spec": "elasticsearch.sn2ne.large" }, "kibanaConfiguration": { "disk": 20, "amount": 1, "diskType": "cloud_ssd", "spec": "elasticsearch.n4.small" }, "masterConfiguration": { "disk": 20, "amount": 3, "diskType": "cloud_ssd", "spec": "elasticsearch.sn2ne.large" }, "networkConfig": { "vswitchId": "vsw-def", "vsArea": "cn-hangzhou-e", "vpcId": "vpc-abc", "type": "vpc" }, "nodeSpec": { "disk": 50, "diskType": "cloud_ssd", "diskEncryption": false, "spec": "elasticsearch.n4.small" } } } 
   
```

## Error code

For a list of error codes, visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

