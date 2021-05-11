# ListInstance

Call the ListInstance to display the details of all instances in a list.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListInstance&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters, and does not involve special request headers. For more information, see the topic about common parameters.

## Request syntax

```
GET /openapi/instances HTTP/1.1
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|page|Integer|Query|No|1|The page number of the returned page.

Start value: **1** . Default value: **1** . |
|size|Integer|Query|No|10|The number of entries to return on each page. Default value: **20** . |
|description|String|Query|No|es-cn-abc|The name of the instance. You can specify a keyword to match multiple instances. For example, search **abc** may return all instances of **abc**, **abcde**,**xyabc**,**xabcy**. |
|instanceId|String|Query|No|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|esVersion|String|Query|No|5.3\_with\_X-Pack|The version of the instance. |
|resourceGroupId|String|Query|No|rg-aekzvowej3i\*\*\*\*|The ID of the resource group to which the ECS instance belongs. |
|tags|String|Query|No|dev-env|Details about the tags. |
|vpcId|String|Query|No|vpc-bp16k1dvzxtmagcva\*\*\*\*|The Virtual Private Cloud of the ID. where the instance resides. |
|zoneId|String|Query|No|cn-hangzhou-i|The ID of the zone where the instance resides. |
|paymentType|String|Query|No|postpaid|The billing method of the instance. Valid values: postpaid \(pay-as-you-go\) and prepaid \(subscription\). |
|instanceCategory|String|Query|No|advanced|The version of the instance. Valid values: x-pack \(commercial edition\) and advanced \(Enhanced Edition\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|10|The number of instances that match the query string. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Instance| |The return results. |
|advancedDedicateMaster|Boolean|false|Indicates whether the instance contains dedicated master nodes. |
|clientNodeConfiguration|Struct| |Coordinates node configuration. |
|amount|Integer|3|The number of cores used for calculation. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_efficiency|The storage type of the node. Only ultra disks \(cloud\_efficiency\) are supported. |
|spec|String|elasticsearch.sn2ne.large|The node specifications of the cluster. |
|createdAt|String|2018-07-13T03:58:07.253Z|The time when the instance was created. |
|dedicateMaster|Boolean|false|Indicates whether the instance contains dedicated master nodes. This parameter is only supported by earlier Elasticsearch versions. |
|description|String|es-cn-abc|The name of the instance. |
|elasticDataNodeConfiguration|Struct| |The configuration of the elastic data node. |
|amount|Integer|3|The number of cores used for calculation. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskEncryption|Boolean|true|Specifies whether to enable cloud disk encryption for the node. |
|diskType|String|cloud\_ssd|The storage type of the node. Supported options: cloud\_ssd \(standard SSD\), cloud\_essd \(Enhanced SSD\), and cloud\_efficiency \(ultra disk\). |
|spec|String|elasticsearch.sn2ne.large|The node specifications of the cluster. |
|esVersion|String|5.5.3\_with\_X-Pack|The version of the instance. |
|extendConfigs|List|\[\{ "configType": "aliVersion", "aliVersion": "ali1.3.0" \}\]|The configuration of the cluster extension parameter. |
|instanceId|String|es-cn-abc|The ID of the Elasticsearch instance. |
|kibanaConfiguration|Struct| |The configuration of Kibana nodes. |
|amount|Integer|1|The number of DRDS server nodes. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|masterConfiguration|Struct| |The configuration of dedicated master nodes. |
|amount|Integer|3|The number of cores used for calculation. |
|disk|Integer|20|The storage space of the node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. Only cloud\_ssd is supported. |
|spec|String|elasticsearch.sn2ne.large|The node specifications of the cluster. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|The type of the network. Only Virtual Private Cloud \(VPC\) is supported. |
|vpcId|String|vpc-abc|The ID of the VPC. |
|vsArea|String|cn-hangzhou-e|The zone where the instance is deployed. |
|vswitchId|String|vsw-def|The ID of the vSwitch. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration of data nodes. |
|disk|Integer|50|The storage size of the node. Unit: GB. |
|diskEncryption|Boolean|false|Indicates whether to use disk encryption. Valid values:

-   true
-   false |
|diskType|String|cloud\_ssd|The storage type of the node. Supported options: cloud\_ssd\(SSD cloud disks\) and cloud\_efficiency \(ultra cloud disks\). |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|paymentType|String|postpaid|The billing method of the physical connection.

Supported: **prepaid** \(Subscription\) and **postpaid** \(Pay-as-you-go\). |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|The ID of the resource group to which the cluster belongs. |
|status|String|active|The status of the cluster.

Supported: **active** \(Normal\), **activating** \(In effect\), **inactive** \(Frozen\) and **invalid** \(Invalidation\). |
|tags|Array of Tag| |Details about the tags. |
|tagKey|String|env|The key of the tag. |
|tagValue|String|dev|The value of the tag. |
|updatedAt|String|2018-07-18T10:10:04.484Z|The time when the instance was last updated. |

**Note:** In the following return example, only the parameters in the returned data list are guaranteed to be included in this article, and the parameters that are not mentioned are for reference only. It is not mandatory to obtain these parameters in the program.

## Examples

Sample requests

```
GET /openapi/instances?description=abc&page=1&size=10
```

Sample success responses

`JSON` Format

```
{
    "Result": [
        {
            "instanceId": "es-cn-n6w1ptcb30009****",
            "version": "5.5.3_with_X-Pack",
            "description": "es-cn-n6w1ptcb30009****",
            "nodeAmount": 3,
            "paymentType": "postpaid",
            "status": "active",
            "privateNetworkIpWhiteList": [],
            "enablePublic": false,
            "nodeSpec": {
                "spec": "elasticsearch.n4.small",
                "disk": 20,
                "diskType": "cloud_ssd",
                "diskEncryption": false
            },
            "networkConfig": {
                "vpcId": "vpc-bp16k1dvzxtmagcva****",
                "vswitchId": "vsw-bp1k4ec6s7sjdbudw****",
                "vsArea": "cn-hangzhou-i",
                "type": "vpc"
            },
            "createdAt": "2020-06-28T08:25:52.895Z",
            "updatedAt": "2020-06-28T08:25:52.895Z",
            "commodityCode": "elasticsearch",
            "extendConfigs": [
                {
                    "configType": "usageScenario",
                    "value": "general"
                },
                {
                    "configType": "maintainTime",
                    "maintainStartTime": "02:00Z",
                    "maintainEndTime": "06:00Z"
                }
            ],
            "endTime": 4749033600000,
            "clusterTasks": [],
            "resourceGroupId": "rg-acfm2h5vbzd****",
            "zoneCount": 1,
            "protocol": "HTTP",
            "zoneInfos": [
                {
                    "zoneId": "cn-hangzhou-i",
                    "status": "NORMAL"
                }
            ],
            "instanceType": "elasticsearch",
            "inited": true,
            "tags": [],
            "esVersion": "5.5.3_with_X-Pack",
            "esIPWhitelist": [],
            "esIPBlacklist": [],
            "kibanaIPWhitelist": [],
            "kibanaPrivateIPWhitelist": [],
            "publicIpWhitelist": [],
            "haveKibana": true,
            "instanceCategory": "x-pack",
            "dedicateMaster": false,
            "advancedDedicateMaster": false,
            "masterConfiguration": {},
            "haveClientNode": false,
            "warmNode": false,
            "warmNodeConfiguration": {},
            "clientNodeConfiguration": {},
            "kibanaConfiguration": {
                "spec": "elasticsearch.n4.small",
                "amount": 1,
                "disk": 0
            },
            "dictList": [],
            "synonymsDicts": [],
            "ikHotDicts": [],
            "aliwsDicts": [],
            "haveGrafana": false,
            "haveCerebro": false,
            "enableKibanaPublicNetwork": false,
            "enableKibanaPrivateNetwork": false,
            "advancedSetting": {
                "gcName": "CMS"
            }
        }
    ],
    "RequestId": "83EC620F-70A2-4497-A6A3-B1DF359D****",
    "Headers": {
        "X-Total-Count": 1
    }
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

