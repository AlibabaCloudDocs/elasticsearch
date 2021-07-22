# DescribeInstance

调用DescribeInstance，查询指定实例的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-3h4k3axh33th9\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Result|Struct| |返回结果。 |
|advancedDedicateMaster|Boolean|true|是否包含专有主节点。取值含义如下：

 -   true：包含。
-   false：不包含。 |
|advancedSetting|Struct| |高级配置。 |
|gcName|String|CMS|GC垃圾回收器名称。支持CMS、G1。 |
|aliwsDicts|Array of Dict| |阿里分词词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|aliws\_ext\_dict.txt|词典文件名称。 |
|sourceType|String|OSS|词典文件来源类型，取值含义如下：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）。
-   ORIGIN：开源Elasticsearch。
-   UPLOAD：上传的文件。 |
|type|String|ALI\_WS|词典文件类型，取值含义如下：

 -   STOP：停用词。
-   MAIN：主词典。
-   SYNONYMS：同义词词典。
-   ALI\_WS：阿里词典。 |
|clientNodeConfiguration|Struct| |协调节点配置信息。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|40|节点存储空间大小，单位：GB。 |
|diskType|String|cloud\_efficiency|节点存储类型，只支持高效云盘（cloud\_efficiency）。 |
|spec|String|elasticsearch.n4.small|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|createdAt|String|2018-07-13T03:58:07.253Z|实例创建时间。 |
|dedicateMaster|Boolean|false|专有主节点（已废弃）。 |
|description|String|es-cn-abc|实例名称。 |
|dictList|Array of DictList| |IK词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|词典文件来源类型，取值含义如下：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）。
-   ORIGIN：开源Elasticsearch。
-   UPLOAD：上传的文件。 |
|type|String|MAIN|词典文件类型，取值含义如下：

 -   STOP：停用词。
-   MAIN：主词典。
-   SYNONYMS：同义词词典。
-   ALI\_WS：阿里词典。 |
|domain|String|es-cn-3h4k3axh33th9\*\*\*\*.elasticsearch.aliyuncs.com|实例的内网地址。 |
|elasticDataNodeConfiguration|Struct| |弹性数据节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位：GB。 |
|diskEncryption|Boolean|true|是否为节点开启云盘加密，取值含义如下：

 -   true：开启。
-   flase：不开启。 |
|diskType|String|cloud\_ssd|节点存储类型。支持：

 -   cloud\_ssd：SSD云盘。
-   cloud\_essd：ESSD云盘。
-   cloud\_efficiency：高效云盘。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|enableKibanaPrivateNetwork|Boolean|false|是否开启Kibana私网访问。取值含义如下：

 -   true：开启。
-   false：不开启。 |
|enableKibanaPublicNetwork|Boolean|true|是否开启Kibana公网访问。取值含义如下：

 -   true：开启。
-   false：不开启。 |
|enablePublic|Boolean|true|是否开启实例的公网地址。取值含义如下：

 -   true：开启。
-   false：不开启。 |
|esConfig|Map|\{"http.cors.allow-credentials":"false"\}|实例的YML文件配置信息。 |
|esIPBlacklist|List|\[ "0.0.0.0/0" \]|私网访问黑名单（已废弃）。 |
|esIPWhitelist|List|\[ "0.0.0.0/0" \]|私网访问白名单（已废弃）。 |
|esVersion|String|6.3.2\_with\_X-Pack|实例版本。 |
|extendConfigs|List|\[\{ "configType": "aliVersion","aliVersion": "ali1.3.0" \}\]|实例的扩展配置。 |
|haveClientNode|Boolean|true|是否包含协调节点。取值含义如下：

 -   true：包含。
-   false：不包含。 |
|haveKibana|Boolean|true|是否包含Kibana节点。取值含义如下：

 -   true：包含。
-   false：不包含。 |
|instanceId|String|es-cn-3h4k3axh33th9\*\*\*\*|实例ID。 |
|isNewDeployment|Boolean|true|是否为新部署架构。 |
|kibanaConfiguration|Struct| |Kibana节点的配置信息。 |
|amount|Integer|1|节点数量。 |
|spec|String|elasticsearch.n4.small|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|kibanaDomain|String|es-cn-3h4k3axh33th9\*\*\*\*.kibana.elasticsearch.aliyuncs.com|Kibana地址。 |
|kibanaIPWhitelist|List|\[ "0.0.0.0/0" \]|Kibana公网地址访问白名单列表。 |
|kibanaPort|Integer|5601|Kibana的访问端口。 |
|kibanaPrivateIPWhitelist|List|\["192.168.XX.XX"\]|Kibana私网地址访问白名单列表。 |
|masterConfiguration|Struct| |Master节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|40|节点存储空间大小，单位：GB。 |
|diskType|String|cloud\_ssd|节点存储类型。只支持cloud\_ssd（SSD云盘）。 |
|spec|String|elasticsearch.n4.small|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，只支持专有网络VPC（Virtual Private Cloud）。 |
|vpcId|String|vpc-abc|VPC网络ID。 |
|vsArea|String|cn-hangzhou-b|实例所在的可用区。 |
|vswitchId|String|vsw-abc|虚拟交换机ID。 |
|whiteIpGroupList|Array of whiteIpGroupList| |白名单组列表。 |
|groupName|String|default|白名单组的组名。默认包含default分组。 |
|ips|List|\["0.0.0.0", "127.0.XX.XX"\]|白名单组中的IP列表。 |
|whiteIpType|String|PRIVATE\_ES|白名单类型。取值含义如下：

 -   PRIVATE\_ES：Elasticsearch私网。
-   PUBLIC\_ES：Elasticsearch公网。
-   PRIVATE\_KIBANA：Kibana私网。
-   PUBLIC\_KIBANA：Kibana公网。 |
|nodeAmount|Integer|2|实例的数据节点数量。 |
|nodeSpec|Struct| |数据节点配置信息。 |
|disk|Integer|0|节点存储空间大小，单位：GB。 |
|diskEncryption|Boolean|true|是否开启云盘加密：

 -   true：开启。
-   false：不开启。 |
|diskType|String|cloud\_ssd|节点磁盘类型。支持：cloud\_ssd（SSD云盘）、cloud\_efficiency（高效云盘）。 |
|spec|String|elasticsearch.n4.small|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|paymentType|String|postpaid|实例的付费方式。支持：

 -   prepaid：包年包月。
-   postpaid：按量付费。 |
|port|Integer|9200|实例的访问端口。 |
|postpaidServiceStatus|String|active|预付费实例叠加的后付费服务状态。取值含义如下：

 -   active：正常。
-   closed：关闭。
-   indebt：欠费冻结中。 |
|privateNetworkIpWhiteList|List|0.0.0.0/0|实例的私网地址访问白名单列表。 |
|protocol|String|HTTP|访问协议。支持：HTTP和HTTPS。 |
|publicDomain|String|es-cn-3h4k3axh33th9\*\*\*\*.elasticsearch.aliyuncs.com|实例的公网地址。 |
|publicIpWhitelist|List|\[ "0.0.0.0/0" \]|实例的公网地址访问白名单列表。 |
|publicPort|Integer|9200|实例的公网访问端口。 |
|resourceGroupId|String|rg-aekzvowej3i\*\*\*\*|实例所属的资源组ID。 |
|serviceVpc|Boolean|true|是否为服务vpc。 |
|status|String|active|实例的状态，取值含义人如下：

 -   active：正常。
-   activating：生效中。
-   inactive：冻结。
-   invalid：失效。 |
|synonymsDicts|Array of SynonymsDicts| |同义词词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|来源类型。 |
|type|String|STOP|词典类型。取值含义如下：

 -   STOP：停用词。
-   MAIN：主词典。
-   SYNONYMS：同义词词典。
-   ALI\_WS：阿里词典。 |
|tags|Array of Tag| |实例标签。 |
|tagKey|String|env|标签键。 |
|tagValue|String|dev|标签值。 |
|updatedAt|String|2018-07-13T03:58:07.253Z|实例最后更新时间。 |
|vpcInstanceId|String|vpc-bp1uag5jj38c\*\*\*\*|专有网络ID。 |
|warmNode|Boolean|true|是否开启冷数据节点。取值含义如下：

 -   true：开启。
-   false：不开启。 |
|warmNodeConfiguration|Struct| |冷数据节点配置信息。 |
|amount|Integer|6|节点数量。 |
|disk|Integer|500|节点存储空间大小，单位：GB。 |
|diskEncryption|Boolean|true|是否开启云盘加密。取值含义如下：

 -   true：开启。
-   false：不开启。 |
|diskType|String|cloud\_efficiency|节点存储空间类型。只支持cloud\_efficiency（高效云盘）。 |
|spec|String|elasticsearch.n4.small|节点规格。规格信息可通过[产品规格](~~271718~~)查看。 |
|zoneCount|Integer|2|实例的可用区个数。 |
|zoneInfos|Array of ZoneInfo| |可用区信息。 |
|status|String|NORMAL|可用区状态。支持：ISOLATION（下线）和NORMAL（正常）。 |
|zoneId|String|cn-hangzhou-b|可用区ID。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |

**说明：** 以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
GET /openapi/instances/es-cn-3h4k3axh33th9**** HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
    "Result": {
        "instanceId": "es-cn-3h4k3axh33th9****",
        "domain": "es-cn-3h4k3axh33th9****.elasticsearch.aliyuncs.com",
        "description": "es-cn-3h4k3axh33th9****",
        "nodeAmount": 2,
        "paymentType": "postpaid",
        "status": "active",
        "port": 9200,
        "esVersion": "6.3.2_with_X-Pack",
        "instanceCategory": "x-pack",
        "esConfig": {
            "action.destructive_requires_name": "true",
            "xpack.security.audit.outputs": "index",
            "xpack.watcher.enabled": "false",
            "xpack.security.audit.enabled": "false",
            "action.auto_create_index": "+.*,-*"
        },
        "esIPWhitelist": [
            "0.0.0.0/0"
        ],
        "esIPBlacklist": [],
        "privateNetworkIpWhiteList": [
            "0.0.0.0/0"
        ],
        "kibanaIPWhitelist": [
            "0.0.0.0/0",
            "::/0"
        ],
        "publicIpWhitelist": [],
        "kibanaDomain": "es-cn-3h4k3axh33th9****.kibana.elasticsearch.aliyuncs.com",
        "kibanaPort": 5601,
        "enablePublic": false,
        "haveKibana": true,
        "nodeSpec": {
            "spec": "elasticsearch.sn1ne.large",
            "disk": 21,
            "diskType": "cloud_ssd"
        },
        "serviceVpc": true,
        "networkConfig": {
            "vpcId": "vpc-bp13n1j3fcv1cqfkg****",
            "vswitchId": "vsw-bp12q2vrqvko0ilg8****",
            "vsArea": "cn-hangzhou-h",
            "type": "vpc",
            "whiteIpGroupList": [
                {
                    "groupName": "default",
                    "whiteIpType": "PRIVATE_ES",
                    "ips": [
                        "0.0.0.0",
                        "127.0.XX.XX"
                    ]
                }
            ]
        },
        "createdAt": "2019-08-28T07:48:59.736Z",
        "updatedAt": "2019-08-28T08:11:33.229Z",
        "inited": true,
        "dedicateMaster": false,
        "advancedDedicateMaster": false,
        "masterConfiguration": {},
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
        "endTime": 4722681600000,
        "dictList": [
            {
                "name": "SYSTEM_MAIN.dic",
                "fileSize": 2782602,
                "type": "MAIN",
                "sourceType": "ORIGIN"
            },
            {
                "name": "SYSTEM_STOPWORD.dic",
                "fileSize": 132,
                "type": "STOP",
                "sourceType": "ORIGIN"
            }
        ],
        "synonymsDicts": [],
        "ikHotDicts": [],
        "aliwsDicts": [],
        "clusterTasks": [],
        "vpcInstanceId": "es-cn-3h4k3axh33th9****-worker",
        "resourceGroupId": "rg-acfm3ghp32r****",
        "zoneCount": 1,
        "protocol": "HTTP",
        "haveGrafana": false,
        "haveCerebro": false,
        "zoneInfos": [
            {
                "zoneId": "cn-hangzhou-h",
                "status": "NORMAL"
            }
        ],
        "enableKibanaPublicNetwork": true,
        "advancedSetting": {
            "gcName": "CMS"
        }
    },
    "RequestId": "68104CC2-DA27-446D-8049-A024F0D4EFEC"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

