# UpdateWhiteIps

Call the UpdateWhiteIps to update the VPC private access whitelist of a Elasticsearch instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateWhiteIps&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters, and does not involve special request headers. For more information, see the topic about common parameters.

## Request syntax

```
PATCH|POST /openapi/instances/[InstanceId]/white-ips HTTP/1.1   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-npk2154oi000b\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|esIPWhitelist|List<String\>|Body|No|\["10.61.xx.xx", "106.11.xx.xx”\]|The list of IP addresses in the whitelist. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|8D58B014-BBD7-4D80-B219-00B9D5C6860C|The ID of the request. |
|Result|Struct| |The return results. |
|esIPWhitelist|List|\["106.11.xx.xx", "10.61.xx.xx"\]|The updated whitelist. |

**Note:** The returned data also contains other information of the instance. For more information, see [ListLogstash](~~160534~~).

## Examples

Sample requests

```
PATCH /openapi/instances/es-cn-npk2154oi000b****/white-ips?esIPWhitelist="[\" 10.61.xx.xx\", \" 106.11.xx.xx\"]" HTTP/1.1 
common request header
```

Sample success responses

`JSON` format

```
{
  "Result": {
    "instanceId": "es-cn-npk2154oi000b****",
    "version": "6.7.0_with_X-Pack",
    "description": "es-cn-npk2154oi000b****",
    "nodeAmount": 3,
    "paymentType": "prepaid",
    "status": "active",
    "privateNetworkIpWhiteList": [
      "106.11.xx.xx",
      "10.61.xx.xx"
    ],
    "enablePublic": false,
    "nodeSpec": {
      "spec": "elasticsearch.sn2ne.large",
      "disk": 23,
      "diskType": "cloud_ssd",
      "diskEncryption": false
    },
    "networkConfig": {
      "vpcId": "vpc-bp12t9s2wbajnm6wr****",
      "vswitchId": "vsw-bp1ndheong7s2dio7****",
      "vsArea": "cn-hangzhou-g",
      "type": "vpc"
    },
    "createdAt": "2021-02-03T13:22:02.309Z",
    "updatedAt": "2021-02-14T07:33:22.686Z",
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
    "clusterTasks": [],
    "vpcInstanceId": "es-cn-npk2154oi000b****-worker",
    "resourceGroupId": "rg-acfm2h5vbzd****",
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
    "domain": "es-cn-npk2154oi000b****.elasticsearch.aliyuncs.com",
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
      "106.11.xx.xx",
      "10.61.xx.xx"
    ],
    "esIPBlacklist": [],
    "kibanaProtocol": "HTTPS",
    "kibanaIPWhitelist": [
      "::1",
      "127.0.0.1"
    ],
    "kibanaPrivateIPWhitelist": [],
    "publicIpWhitelist": [],
    "kibanaDomain": "es-cn-npk2154oi000b****.kibana.elasticsearch.aliyuncs.com",
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
        "name": "dict_0.dic",
        "fileSize": 220,
        "sourceType": "ORIGIN",
        "type": "MAIN"
      },
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
    "synonymsDicts": [
      {
        "name": "aliws_ext_dict.txt",
        "fileSize": 17,
        "sourceType": "ORIGIN",
        "type": "SYNONYMS"
      }
    ],
    "ikHotDicts": [
      {
        "name": "dict_0.dic",
        "fileSize": 220,
        "sourceType": "ORIGIN",
        "type": "MAIN"
      }
    ],
    "aliwsDicts": [],
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
  "RequestId": "8D58B014-BBD7-4D80-B219-00B9D5C6860C"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the instance cannot be found. Check the status of the instance.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

