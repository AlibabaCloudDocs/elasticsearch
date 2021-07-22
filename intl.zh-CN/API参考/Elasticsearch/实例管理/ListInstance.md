# ListInstance

调用ListInstance，在列表中展示所有实例的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|page|Integer|Query|否|1|实例列表的页码。

 起始值：**1**，默认值：**1**。 |
|size|Integer|Query|否|10|分页查询时设置的每页条数。最大值：**100**，默认值：**10**。 |
|description|String|Query|否|aliyunes\_test1|实例名称，支持模糊查询。例如搜索**abc**的所有实例，则可能返回**abc**、**abcde**、**xyabc**、**xabcy**的所有实例。 |
|instanceId|String|Query|否|es-cn-v641a0ta3000g\*\*\*\*|实例ID。 |
|esVersion|String|Query|否|6.7.0\_with\_X-Pack|实例版本。 |
|resourceGroupId|String|Query|否|rg-aekzvowej3i\*\*\*\*|实例所在的资源组ID。 |
|tags|String|Query|否|dev-env|实例标签。 |
|vpcId|String|Query|否|vpc-bp16k1dvzxtmagcva\*\*\*\*|实例所在的专有网络ID。 |
|zoneId|String|Query|否|cn-hangzhou-i|实例所在的可用区ID。 |
|paymentType|String|Query|否|postpaid|实例的付费类型。可选值：

 -   postpaid（按量付费）
-   prepaid（包年包月） |
|instanceCategory|String|Query|否|advanced|实例版本。可选值：

 -   x-pack：商业版。
-   advanced：增强版。
-   community：基础版本。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|10|实例总记录数。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Instance| |返回结果。 |
|advancedDedicateMaster|Boolean|false|是否包含专有主节点，取值含义如下：

 -   true：包含。
-   flase：不包含。 |
|clientNodeConfiguration|Struct| |协调节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_efficiency|节点的存储类型，只支持高效云盘（cloud\_efficiency）。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|createdAt|String|2018-07-13T03:58:07.253Z|实例创建时间。 |
|dedicateMaster|Boolean|false|是否包含专有主节点（已废弃），取值含义如下：

 -   true：包含。
-   flase：不包含。 |
|description|String|es-cn-abc|实例名称。 |
|elasticDataNodeConfiguration|Struct| |弹性数据节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskEncryption|Boolean|true|是否为节点开启云盘加密，取值含义如下：

 -   true：开启。
-   flase：不开启。 |
|diskType|String|cloud\_ssd|节点存储类型。支持：

 -   cloud\_ssd：SSD云盘。
-   cloud\_essd：ESSD云盘。
-   cloud\_efficiency：高效云盘。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|esVersion|String|6.7.0\_with\_X-Pack|实例版本。 |
|extendConfigs|List|\[\{ "configType": "aliVersion", "aliVersion": "ali1.3.0" \}\]|集群扩展参数配置。 |
|instanceId|String|es-cn-v641a0ta3000g\*\*\*\*|实例ID。 |
|isNewDeployment|String|true|是否为新部署架构。 |
|kibanaConfiguration|Struct| |Kibana节点配置。 |
|amount|Integer|1|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点存储类型。 |
|spec|String|elasticsearch.n4.small|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|masterConfiguration|Struct| |Master节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位为GB。 |
|diskType|String|cloud\_ssd|节点存储类型。只支持cloud\_ssd（SSD云盘）。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，只支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-abc|专有网络ID。 |
|vsArea|String|cn-hangzhou-e|实例所在的可用区。 |
|vswitchId|String|vsw-def|虚拟交换机ID。 |
|nodeAmount|Integer|2|实例的数据节点数量。 |
|nodeSpec|Struct| |数据节点配置信息。 |
|disk|Integer|50|节点的存储空间大小，单位为GB。 |
|diskEncryption|Boolean|false|是否使用磁盘加密，取值含义如下：

 -   true：使用。
-   false：不使用。 |
|diskType|String|cloud\_ssd|节点的存储类型。支持：

 -   cloud\_ssd：SSD云盘。
-   cloud\_efficiency：高效云盘。 |
|spec|String|elasticsearch.n4.small|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|paymentType|String|postpaid|实例的付费方式。支持：

 -   **prepaid**：包年包月。
-   **postpaid**：按量付费。 |
|postpaidServiceStatus|String|active|预付费实例叠加的后付费服务状态。支持：

 -   **active**：正常。
-   **closed**：关闭。
-   **indebt**：欠费冻结中。 |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|资源组ID。 |
|serviceVpc|Boolean|true|是否为服务vpc。 |
|status|String|active|实例的状态，取值含义如下：

 -   active：正常。
-   activating：生效中。
-   inactive：冻结。
-   invalid：失效。 |
|tags|Array of Tag| |实例标签。 |
|tagKey|String|env|标签键。 |
|tagValue|String|dev|标签值。 |
|updatedAt|String|2018-07-18T10:10:04.484Z|实例最后更新的时间。 |

**说明：** 以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
GET /openapi/instances?description=abc&page=1&size=10
```

正常返回示例

`JSON`格式

```
{
    "Result": [
        {
            "instanceId": "es-cn-v641a0ta3000g****",
            "description": "aliyunes_test1",
            "nodeAmount": 3,
            "paymentType": "postpaid",
            "status": "active",
            "esVersion": "6.7.0_with_X-Pack",
            "esConfig": {},
            "esIPWhitelist": [],
            "esIPBlacklist": [],
            "privateNetworkIpWhiteList": [],
            "kibanaIPWhitelist": [],
            "publicIpWhitelist": [],
            "serviceVpc": true,
            "enablePublic": false,
            "haveKibana": true,
            "nodeSpec": {
                "spec": "elasticsearch.sn1ne.large",
                "disk": 20,
                "diskType": "cloud_ssd",
                "diskEncryption": true
            },
            "networkConfig": {
                "vpcId": "vpc-bp1xk0naij7jx4ph1****",
                "vswitchId": "vsw-bp1ogpdintii5qvyx****",
                "vsArea": "cn-hangzhou-g",
                "type": "vpc"
            },
            "createdAt": "2019-08-26T08:18:06.652Z",
            "updatedAt": "2019-08-26T08:19:49.448Z",
            "inited": true,
            "tags": [
                {
                    "tagKey": "env",
                    "tagValue": "dev"
                }
            ],
            "dedicateMaster": false,
            "advancedDedicateMaster": true,
            "masterConfiguration": {
                "spec": "elasticsearch.ic5.large",
                "amount": 3,
                "diskType": "cloud_ssd",
                "disk": 20
            },
            "haveClientNode": true,
            "warmNode": true,
            "warmNodeConfiguration": {
                "spec": "elasticsearch.ic5.large",
                "amount": 2,
                "diskType": "cloud_efficiency",
                "disk": 500,
                "diskEncryption": true
            },
            "clientNodeConfiguration": {
                "spec": "elasticsearch.ic5.large",
                "amount": 2,
                "diskType": "cloud_efficiency",
                "disk": 20
            },
            "kibanaConfiguration": {
                "spec": "elasticsearch.n4.small",
                "amount": 1,
                "disk": 0
            },
            "commodityCode": "elasticsearch",
            "endTime": 4722508800000,
            "dictList": [],
            "synonymsDicts": [],
            "ikHotDicts": [],
            "aliwsDicts": [],
            "clusterTasks": [],
            "resourceGroupId": "rg-acfmwriiikz****",
            "zoneCount": 1,
            "protocol": "HTTP",
            "haveGrafana": false,
            "haveCerebro": false,
            "zoneInfos": [
                {
                    "zoneId": "cn-hangzhou-g",
                    "status": "NORMAL"
                }
            ],
            "enableKibanaPublicNetwork": false,
            "advancedSetting": {
                "gcName": "CMS"
            }
        },
        {
            "instanceId": "es-cn-v641920bh0006****",
            "description": "aliyunes_test2",
            "nodeAmount": 2,
            "paymentType": "postpaid",
            "status": "active",
            "esVersion": "6.7.0_with_X-Pack",
            "esConfig": {},
            "esIPWhitelist": [],
            "esIPBlacklist": [],
            "privateNetworkIpWhiteList": [],
            "kibanaIPWhitelist": [],
            "publicIpWhitelist": [],
            "enablePublic": false,
            "haveKibana": true,
            "nodeSpec": {
                "spec": "elasticsearch.sn2ne.2xlarge",
                "disk": 20,
                "diskType": "cloud_ssd",
                "diskEncryption": false
            },
            "networkConfig": {
                "vpcId": "vpc-bp1op7luys63go2x5****",
                "vswitchId": "vsw-bp1rusvg785q97ucp****",
                "vsArea": "cn-hangzhou-i",
                "type": "vpc"
            },
            "createdAt": "2019-08-07T13:14:07.974Z",
            "updatedAt": "2019-08-12T03:04:27.215Z",
            "inited": true,
            "tags": [],
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
            "commodityCode": "elasticsearch",
            "endTime": 4720867200000,
            "dictList": [],
            "synonymsDicts": [],
            "ikHotDicts": [],
            "aliwsDicts": [],
            "clusterTasks": [],
            "resourceGroupId": "rg-acfmwriiikz****",
            "zoneCount": 1,
            "protocol": "HTTP",
            "haveGrafana": false,
            "haveCerebro": false,
            "zoneInfos": [
                {
                    "zoneId": "cn-hangzhou-i",
                    "status": "NORMAL"
                }
            ],
            "enableKibanaPublicNetwork": false,
            "advancedSetting": {
                "gcName": "CMS"
            }
        }
    ]
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

