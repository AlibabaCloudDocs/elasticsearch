# UpdateKibanaWhiteIps

调用UpdateKibanaWhiteIps，更新指定阿里云Elasticsearch实例的Kibana访问白名单。

**说明：** 调用该接口时，当实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法更新信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdateKibanaWhiteIps&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/kibana-white-ips HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-i7m26sknb000k\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不值过64个ASCII字符。 |

RequestBody中还需填入以下参数，用来指定迁移信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|kibanaIPWhitelist

|List<String\\\>

|是

|\["0.0.0.0/0", "1.1.XX.XX"\]

|待更新的Kibana访问白名单。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|E5EF11F1-DBAE-4020-AC24-DFA6C434\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|kibanaIPWhitelist|List|\["0.0.0.0/0", "1.1.XX.XX"\]|Kibana访问白名单列表。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-i7m26sknb000k****/kibana-white-ips HTTP/1.1
公共请求头
{"kibanaIPWhitelist": ["0.0.0.0/0", "1.1.XX.XX"]}
```

正常返回示例

`JSON`格式

```
{
  "Result": {
    "instanceId": "es-cn-i7m27ausp001l****",
    "version": "7.10.0_with_X-Pack",
    "description": "lrr",
    "nodeAmount": 3,
    "paymentType": "postpaid",
    "status": "active",
    "privateNetworkIpWhiteList": [
      "0.0.0.0/0"
    ],
    "enablePublic": false,
    "nodeSpec": {
      "spec": "elasticsearch.sn1ne.large",
      "disk": 20,
      "diskType": "cloud_essd",
      "diskEncryption": false,
      "performanceLevel": "PL1"
    },
    "dataNode": true,
    "networkConfig": {
      "vpcId": "vpc-bp1jy348ibzulk6hn****",
      "vswitchId": "vsw-bp1a0mifpletdd1da****",
      "vsArea": "cn-hangzhou-h",
      "type": "vpc"
    },
    "createdAt": "2021-06-03T06:55:39.836Z",
    "updatedAt": "2021-06-03T06:55:39.836Z",
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
      },
      {
        "configType": "aliVersion",
        "aliVersion": "ali1.4.0"
      }
    ],
    "endTime": 4778409600000,
    "clusterTasks": [],
    "vpcInstanceId": "es-cn-i7m27ausp001l****-worker",
    "resourceGroupId": "rg-acfmxxkk2p7****",
    "zoneCount": 1,
    "protocol": "HTTP",
    "zoneInfos": [
      {
        "zoneId": "cn-hangzhou-h",
        "status": "NORMAL"
      }
    ],
    "instanceType": "elasticsearch",
    "inited": true,
    "tags": [
      {
        "tagKey": "acs:rm:rgId",
        "tagValue": "rg-acfmxxkk2p7mxky"
      }
    ],
    "domain": "es-cn-i7m27ausp001l****.elasticsearch.aliyuncs.com",
    "port": 9200,
    "esVersion": "7.10.0_with_X-Pack",
    "esConfig": {
      "action.destructive_requires_name": "true",
      "xpack.watcher.enabled": "false",
      "action.auto_create_index": "+.*,-*"
    },
    "esIPWhitelist": [
      "0.0.0.0/0"
    ],
    "esIPBlacklist": [],
    "kibanaProtocol": "HTTPS",
    "kibanaIPWhitelist": [
      "0.0.0.0/0",
      "1.1.XX.XX"
    ],
    "kibanaPrivateIPWhitelist": [],
    "publicIpWhitelist": [],
    "kibanaDomain": "es-cn-i7m27ausp001l****.kibana.elasticsearch.aliyuncs.com",
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
    "aliwsDicts": [],
    "haveGrafana": false,
    "haveCerebro": false,
    "enableKibanaPublicNetwork": true,
    "enableKibanaPrivateNetwork": false,
    "advancedSetting": {
      "gcName": "CMS"
    },
    "enableMetrics": false,
    "readWritePolicy": {
      "writeHa": false
    }
  },
  "RequestId": "A041337B-68BE-4C10-8B81-6757D0EA8DBD"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

