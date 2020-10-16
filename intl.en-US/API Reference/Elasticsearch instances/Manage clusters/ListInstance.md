# ListInstance

You can call this operation to list all specified instances and display their details.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListInstance&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|page|Integer|No|1|The number of the page to return.

Starting value: **1**. Default value: **1**. |
|size|Integer|No|10|The number of entries to return on each page.

Maximum Value: **100**. Default value: **10**. |
|description|String|No|es-cn-abc|The name of the instance. You can specify a keyword to match multiple instances. For example, if you specify **abc**, instances whose names are **abc**, **abcde**, **xyabc**, and **xabcy** are matched. |
|instanceId|String|No|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|esVersion|String|No|5.3\_with\_X-Pack|The version of the instance. |
|resourceGroupId|String|No|rg-aekzvowej3i\*\*\*\*|The ID of the resource group to which the instance belongs. |
|tags|String|No|dev-env|The tags of the instances. |
|vpcId|String|No|vpc-bp16k1dvzxtmagcva\*\*\*\*|The ID of the VPC to which the instance belongs. |
|zoneId|String|No|cn-hangzhou-i|The ID of the zone to which the instance belongs. |
|paymentType|String|No|postpaid|The billing method of the instance. Valid values: postpaid and prepaid. |
|instanceCategory|String|No|advanced|The version of the instance. Valid values: x-pack \(commercial edition\) and advanced \(Enhanced Edition\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|10|The number of instances that match the query string. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Instance| |The return results. |
|advancedDedicateMaster|Boolean|false|Indicates whether the instance contains dedicated master nodes. |
|clientNodeConfiguration|Struct| |The client node configuration. |
|amount|Integer|3|The number of nodes in the cluster. |
|disk|Integer|20|The size of the node storage space. Unit: GB. |
|diskType|String|cloud\_efficiency|The storage type of the node. Only ultra disks are supported \(cloud\_efficiency\). |
|spec|String|elasticsearch.sn2ne.large|The node specifications of the cluster. |
|createdAt|String|2018-07-13T03:58:07.253Z|The time when the instance was created. |
|dedicateMaster|Boolean|false|Indicates whether the instance contains dedicated master nodes. This parameter is only supported by earlier Elasticsearch versions. |
|description|String|es-cn-abc|The name of the instance. |
|elasticDataNodeConfiguration|Struct| |The configuration of the elastic data node. |
|amount|Integer|3|The number of nodes in the cluster. |
|disk|Integer|20|The size of the node storage space. Unit: GB. |
|diskEncryption|Boolean|true|Specifies whether to enable disk encryption for the node. |
|diskType|String|cloud\_ssd|The storage type of the node. Support: cloud\_ssd\(SSD cloud disk\), cloud\_essd \(Enhanced SSD\), cloud\_efficiency \(ultra cloud disk\) |
|spec|String|elasticsearch.sn2ne.large|The node specifications of the cluster. |
|esVersion|String|5.5.3\_with\_X-Pack|The version of the instance. |
|instanceId|String|es-cn-abc|The ID of the Elasticsearch instance. |
|kibanaConfiguration|Struct| |The configuration of Kibana nodes. |
|amount|Integer|1|The number of performance metrics. |
|disk|Integer|20|The size of the node storage space. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|masterConfiguration|Struct| |The configuration of dedicated master nodes. |
|amount|Integer|3|The number of nodes in the cluster. |
|disk|Integer|20|The size of the node storage space. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. This tool only supports cloud\_ssd \(cloud SSD\) disks. |
|spec|String|elasticsearch.sn2ne.large|The node specifications of the cluster. |
|networkConfig|Struct| |The network configuration. |
|type|String|vpc|The network type. Only Virtual Private Cloud \(VPC\) is supported. |
|vpcId|String|vpc-abc|The ID of the VPC. |
|vsArea|String|cn-hangzhou-e|The zone where the instance is deployed. |
|vswitchId|String|vsw-def|The ID of the VSwitch associated with the specified VPC. |
|nodeAmount|Integer|2|The number of data nodes. |
|nodeSpec|Struct| |The configuration of data nodes. |
|disk|Integer|50|The storage space size per data node. Unit: GB. |
|diskType|String|cloud\_ssd|The storage type of the node. Valid values: cloud\_ssd and cloud\_efficiency. |
|spec|String|elasticsearch.n4.small|The specification of data nodes. |
|paymentType|String|postpaid|The billing method of the created ECS instance.

Valid values: **prepaid** and **postpaid**. |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|The ID of the resource group. |
|status|String|active|The state of the cluster.

Supported options: **active** \(normal\), **activating** \(initializing\), **inactive** \(blocked\), and **invalid** \(expired\). |
|tags|Array of Tag| |The tags of the instances. |
|tagKey|String|env|The key of the tag. |
|tagValue|String|dev|The value of the tag. |
|updatedAt|String|2018-07-18T10:10:04.484Z|The time when the instance was last updated. |

**Note:** The following response examples may contain the parameters in the list of returned data. These parameters are for reference only. You must make sure that your application is not strongly reliant on these parameters.

## Examples

Sample requests

```
GET /openapi/instances? description=abc&page=1&size=10
```

Sample success responses

`XML` format

```
<Result>
    <instanceId>es-cn-n6w1ptcb30009****</instanceId>
    <version>5.5.3_with_X-Pack</version>
    <description>es-cn-n6w1ptcb30009****</description>
    <nodeAmount>3</nodeAmount>
    <paymentType>postpaid</paymentType>
    <status>active</status>
    <enablePublic>false</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.n4.small</spec>
        <disk>20</disk>
        <diskType>cloud_ssd</diskType>
        <diskEncryption>false</diskEncryption>
    </nodeSpec>
    <networkConfig>
        <vpcId>vpc-bp16k1dvzxtmagcva****</vpcId>
        <vswitchId>vsw-bp1k4ec6s7sjdbudw****</vswitchId>
        <vsArea>cn-hangzhou-i</vsArea>
        <type>vpc</type>
    </networkConfig>
    <createdAt>2020-06-28T08:25:52.895Z</createdAt>
    <updatedAt>2020-06-28T08:25:52.895Z</updatedAt>
    <commodityCode>elasticsearch</commodityCode>
    <extendConfigs>
        <configType>usageScenario</configType>
        <value>general</value>
    </extendConfigs>
    <extendConfigs>
        <configType>maintainTime</configType>
        <maintainStartTime>02:00Z</maintainStartTime>
        <maintainEndTime>06:00Z</maintainEndTime>
    </extendConfigs>
    <endTime>4749033600000</endTime>
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-hangzhou-i</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>elasticsearch</instanceType>
    <inited>true</inited>
    <esVersion>5.5.3_with_X-Pack</esVersion>
    <haveKibana>true</haveKibana>
    <instanceCategory>x-pack</instanceCategory>
    <dedicateMaster>false</dedicateMaster>
    <advancedDedicateMaster>false</advancedDedicateMaster>
    <masterConfiguration/>
    <haveClientNode>false</haveClientNode>
    <warmNode>false</warmNode>
    <warmNodeConfiguration/>
    <clientNodeConfiguration/>
    <kibanaConfiguration>
        <spec>elasticsearch.n4.small</spec>
        <amount>1</amount>
        <disk>0</disk>
    </kibanaConfiguration>
    <haveGrafana>false</haveGrafana>
    <haveCerebro>false</haveCerebro>
    <enableKibanaPublicNetwork>false</enableKibanaPublicNetwork>
    <enableKibanaPrivateNetwork>false</enableKibanaPrivateNetwork>
    <advancedSetting>
        <gcName>CMS
    </advancedSetting>
</Result>
<RequestId>83EC620F-70A2-4497-A6A3-B1DF359D****</RequestId>
<Headers>
    <X-Total-Count>1</X-Total-Count>
</Headers>
```

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

