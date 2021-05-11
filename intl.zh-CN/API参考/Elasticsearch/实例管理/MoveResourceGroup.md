# MoveResourceGroup

调用MoveResourceGroup，迁移实例到指定资源组。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=MoveResourceGroup&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST|PUT /openapi/instances/[InstanceId]/resourcegroup HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定将实例迁移到的资源组信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|resourceGroupId

|String

|是

|rg-acfm2h5vbzd\*\*\*\*

|目标资源组ID。可在[资源组](https://resourcemanager.console.aliyun.com/resource-groups)页面获取。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|createdAt|String|2020-07-06T10:18:48.662Z|实例创建时间。 |
|description|String|es-cn-abc|实例名称。 |
|dictList|Array of dictList| |IK词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|来源类型，支持：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）
-   ORIGIN：保留之前已经上传的词典 |
|type|String|MAIN|词典类型，支持：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|domain|String|es-cn-nif1q8auz0003\*\*\*\*.elasticsearch.aliyuncs.com|实例的内网访问地址。 |
|esVersion|String|6.7.0\_with\_X-Pack|实例版本。 |
|instanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|kibanaConfiguration|Struct| |Kibana节点配置。 |
|amount|Integer|1|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位：GB。 |
|diskType|String|cloud\_ssd|节点存储类型。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|kibanaDomain|String|es-cn-nif1q8auz0003\*\*\*\*.kibana.elasticsearch.aliyuncs.com|Kibana公网访问地址。 |
|kibanaPort|Integer|5601|Kibana公网端口。 |
|masterConfiguration|Struct| |Master节点配置。 |
|amount|Integer|3|节点数量。 |
|disk|Integer|20|节点存储空间大小，单位：GB。 |
|diskType|String|cloud\_ssd|节点存储类型。 |
|spec|String|elasticsearch.sn2ne.large|节点规格。 |
|networkConfig|Struct| |网络配置。 |
|type|String|vpc|网络类型，只支持专有网络。 |
|vpcId|String|vpc-bp16k1dvzxtmagcva\*\*\*\*|专有网络ID。 |
|vsArea|String|cn-hangzhou-i|实例所在的可用区。 |
|vswitchId|String|vsw-bp1k4ec6s7sjdbudw\*\*\*\*|虚拟交换机ID。 |
|nodeAmount|Integer|2|实例的数据节点数量。 |
|nodeSpec|Struct| |数据节点配置信息。 |
|disk|Integer|50|节点的存储空间大小，单位：GB。 |
|diskType|String|cloud\_ssd|节点的存储类型。 |
|spec|String|elasticsearch.n4.small|节点规格。 |
|paymentType|String|postpaid|实例的付费方式。支持：

 -   prepaid：包年包月
-   postpaid：按量付费 |
|publicDomain|String|es-cn-n6w1o1x0w001c\*\*\*\*.public.elasticsearch.aliyuncs.com|公网访问地址。 |
|publicPort|Integer|9200|公网端口。 |
|status|String|active|实例的状态。支持：

 -   active：正常
-   activating：生效中
-   inactive：冻结
-   invalid：失效 |
|synonymsDicts|Array of synonymsDicts| |同义词词典配置。 |
|fileSize|Long|2782602|词典文件大小，单位：字节。 |
|name|String|SYSTEM\_MAIN.dic|词典文件名称。 |
|sourceType|String|ORIGIN|来源类型，支持：

 -   OSS：OSS开放存储（需要确保OSS存储空间为公共可读）
-   ORIGIN：保留之前已经上传的词典 |
|type|String|STOP|词典类型，支持：

 -   STOP：停用词
-   MAIN：主词典
-   SYNONYMS：同义词词典
-   ALI\_WS：阿里词典 |
|updatedAt|String|2018-07-18T10:10:04.484Z|实例最后更新的时间。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/resourcegroup HTTP/1.1
公共请求头
{
   "resourceGroupId": "rg-acfm2h5vbzd****"
}
```

正常返回示例

`JSON`格式

```
{
  "Result": {
    "dryRun": false,
    "id": "24210",
    "instanceId": "es-cn-npk2151ww000a****",
    "version": "6.7.0_with_X-Pack",
    "description": "",
    "ownerId": "168520994880****",
    "nodeAmount": 3,
    "paymentType": "prepaid",
    "status": "inactive",
    "privateNetworkIpWhiteList": [
      "0.0.0.0/0"
    ],
    "enablePublic": false,
    "nodeSpec": {
      "spec": "elasticsearch.sn2ne.2xlarge",
      "disk": 20,
      "diskType": "cloud_ssd",
      "diskEncryption": false,
      "empty": false
    },
    "networkConfig": {
      "vpcId": "vpc-bp12nu14urf0upaf4****",
      "vswitchId": "vsw-bp1xn8mossizozn6q****",
      "vsArea": "cn-hangzhou-g",
      "type": "vpc"
    },
    "lastPayTime": "2021-03-03T16:00:58.000+0000",
    "createdAt": "2021-02-03T12:22:14.643Z",
    "updatedAt": "2021-03-04T03:46:02.358Z",
    "commodityCode": "elasticsearchpre",
    "extendConfigs": [
      {
        "configType": "usageScenario",
        "value": "general"
      },
      {
        "configType": "maintainTime",
        "maintainStartTime": "02:00Z",
        "maintainEndTime": "06:00Z"
      },
      {
        "configType": "aliVersion",
        "aliVersion": "ali1.3.0"
      }
    ],
    "endTime": 1614787200000,
    "force": false,
    "ignoreStatus": false,
    "clusterTasks": [],
    "vpcInstanceId": "es-cn-npk2151ww000a****-worker",
    "resourceGroupId": "rg-aek2wq2jlqd****",
    "zoneCount": 1,
    "protocol": "HTTP",
    "zoneInfos": [
      {
        "zoneId": "cn-hangzhou-g",
        "status": "NORMAL"
      }
    ],
    "instanceType": "elasticsearch",
    "inited": true,
    "tags": [
      {
        "tagKey": "acs:rm:rgId",
        "tagValue": "rg-acfm2h5vbzd****"
      }
    ],
    "customerLabels": [],
    "domain": "es-cn-npk2151ww000a****.elasticsearch.aliyuncs.com",
    "port": 9200,
    "esVersion": "6.7.0_with_X-Pack",
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
    "kibanaProtocol": "HTTPS",
    "kibanaIPWhitelist": [
      "::1",
      "127.0.0.1"
    ],
    "kibanaPrivateIPWhitelist": [],
    "publicIpWhitelist": [],
    "kibanaDomain": "es-cn-npk2151ww000a****.kibana.elasticsearch.aliyuncs.com",
    "kibanaPort": 5601,
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
    "elasticDataNodeConfiguration": {},
    "haveElasticDataNode": false,
    "dictList": [
      {
        "name": "SYSTEM_MAIN.dic",
        "fileSize": 2782602,
        "sourceType": "ORIGIN",
        "type": "MAIN"
      },
      {
        "name": "SYSTEM_STOPWORD.dic",
        "fileSize": 132,
        "sourceType": "ORIGIN",
        "type": "STOP"
      }
    ],
    "synonymsDicts": [],
    "ikHotDicts": [],
    "aliwsDicts": [
      {
        "name": "aliws_ext_dict.txt",
        "fileSize": 17,
        "sourceType": "ORIGIN",
        "type": "ALI_WS"
      }
    ],
    "haveGrafana": false,
    "haveCerebro": false,
    "enableKibanaPublicNetwork": true,
    "enableKibanaPrivateNetwork": false,
    "advancedSetting": {
      "gcName": "CMS"
    },
    "enableMetrics": true,
    "readWritePolicy": {
      "writeHa": false
    }
  },
  "RequestId": "D63D226F-2B80-4BF9-8F70-DBEDCC8DC2AF"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

