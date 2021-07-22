# UpdatePublicWhiteIps

调用UpdatePublicWhiteIps，更新指定Elasticsearch实例的公网地址访问白名单。

**说明：** 调用该接口时，请注意：当实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，无法更新实例的公网地址访问白名单。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpdatePublicWhiteIps&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/public-white-ips HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-tl329rbpc0001\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|modifyMode|String|Query|否|Cover|修改方式，取值含义如下：

 -   Cover（默认值）：使用ips参数的值覆盖原IP白名单。
-   Append：在原IP白名单中增加ips参数中输入的IP地址。
-   Delete：在原IP白名单中删除ips参数中输入的IP地址，至少需要保留一个IP地址。 |

## RequestBody

|参数

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|publicIpWhitelist

|List<String\>

|是

|\["0.0.0.0/0","0.0.0.0/1"\]

|实例公网白名单列表。 |
|whiteIpGroup

|Struct

|否

|\{"groupName": "test\_group\_name","ips": \["0.0.0.0","10.2.XX.XX"\]\}

|以白名单组whiteIpGroup传参方式，更新实例白名单安全配置。仅支持更新一个白名单组。 |
|└ groupName

|String

|whiteIpGroup中必填

|test\_group\_name

|白名单组的组名。 |
|└ ips

|List<String\>

|whiteIpGroup中必填

|\["0.0.0.0", "10.2.XX.XX"\]

|白名单组中的IP列表。 |

**说明：**

modifyMode为Cover时，如果ips为空，则删除该白名单组。

modifyMode为Cover时，如果groupName不在已有白名单组组名的列表中，则会新建一个白名单组。

modifyMode为Delete时，删除后的ips至少需要保留一个IP地址。

modifyMode为Append时，需要保证白名单组组名为已创建的，否则会报NotFound。

白名单组的增加、删除，是由modifyMode为Cover的调用来实现的，Delete和Append无法实现白名单组粒度的增删，只能修改白名单组中的IP列表。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|C82758DD-282F-4D48-934F-92170A33\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|publicIpWhitelist|List|\["0.0.0.0","10.2.XX.XX","110.0.XX.XX/8"\]|公网地址访问白名单列表。 |

**说明：** 下文返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，参数说明请参见[ListInstance](~~142230~~)。程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
POST /openapi/instances/es-cn-tl329rbpc0001****/public-white-ips HTTP/1.1

{
    "publicIpWhitelist": [
        "110.0.XX.XX/8"
    ]
}
  或
{
    "whiteIpGroup": {
        "groupName": "test_group_name", 
        "ips": [
            "0.0.0.0", 
            "10.2.XX.XX"
        ]
    }
}
```

正常返回示例

`JSON`格式

```
{
    "Result": {
        "instanceId": "es-cn-tl329rbpc0001****",
        "version": "7.10.0_with_X-Pack",
        "description": "test",
        "nodeAmount": 0,
        "paymentType": "postpaid",
        "status": "active",
        "privateNetworkIpWhiteList": [
            "11.22.XX.XX",
            "0.0.XX.XX/0"
        ],
        "enablePublic": true,
        "nodeSpec": {},
        "dataNode": false,
        "networkConfig": {
            "vpcId": "vpc-bp1jy348ibzulk6h***",
            "vswitchId": "vsw-bp1a0mifpletdd1da****",
            "vsArea": "cn-hangzhou-h",
            "whiteIpGroupList": [
                {
                    "groupName": "default",
                    "ips": [
                        "0.0.XX.XX/0",
                        "11.22.XX.XX"
                    ],
                    "whiteIpType": "PRIVATE_ES"
                },
                {
                    "groupName": "default",
                    "ips": [
                        "110.0.XX.XX/9"
                    ],
                    "whiteIpType": "PUBLIC_KIBANA"
                },
                {
                    "groupName": "default",
                    "ips": [
                        "192.168.XX.XX/24"
                    ],
                    "whiteIpType": "PRIVATE_KIBANA"
                },
                {
                    "groupName": "default",
                    "ips": [
                        "110.0.XX.XX/8"
                    ],
                    "whiteIpType": "PUBLIC_ES"
                },
                {
                    "groupName": "test_group_name",
                    "ips": [
                        "0.0.0.0",
                        "10.2.XX.XX"
                    ],
                    "whiteIpType": "PUBLIC_ES"
                }
            ],
            "type": "vpc"
        },
        "createdAt": "2021-07-21T01:29:38.510Z",
        "updatedAt": "2021-07-21T06:40:32.438Z",
        "commodityCode": "elasticsearch",
        "extendConfigs": [
            {
                "configType": "usageScenario",
                "value": "log"
            },
            {
                "configType": "maintainTime",
                "maintainStartTime": "02:00Z",
                "maintainEndTime": "06:00Z"
            },
            {
                "configType": "aliVersion",
                "aliVersion": "ali1.4.0"
            },
            {
                "configType": "followCube",
                "followClusterEnabled": true
            }
        ],
        "endTime": 4782556800000,
        "clusterTasks": [],
        "vpcInstanceId": "es-cn-tl329rbpc0001****-worker",
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
                "tagValue": "rg-acfmxxkk2p7****"
            }
        ],
        "serviceVpc": true,
        "domain": "es-cn-tl329rbpc0001****.elasticsearch.aliyuncs.com",
        "port": 9200,
        "esVersion": "7.10.0_with_X-Pack",
        "esConfig": {
            "action.destructive_requires_name": "true",
            "xpack.watcher.enabled": "false",
            "action.auto_create_index": "+.*,-*"
        },
        "esIPWhitelist": [
            "11.22.XX.XX",
            "0.0.XX.XX/0"
        ],
        "esIPBlacklist": [],
        "kibanaProtocol": "HTTPS",
        "kibanaIPWhitelist": [
            "::1",
            "110.0.XX.XX/9"
        ],
        "kibanaPrivateIPWhitelist": [
            "192.168.XX.XX/24"
        ],
        "publicIpWhitelist": [
            "0.0.0.0",
            "10.2.XX.XX",
            "110.0.XX.XX/8"
        ],
        "kibanaDomain": "es-cn-tl329rbpc0001****.kibana.elasticsearch.aliyuncs.com",
        "kibanaPort": 5601,
        "kibanaPrivateDomain": "es-cn-tl329rbpc0001****-kibana.internal.elasticsearch.aliyuncs.com",
        "kibanaPrivatePort": 5601,
        "publicPort": 9200,
        "publicDomain": "es-cn-tl329rbpc0001****.public.elasticsearch.aliyuncs.com",
        "haveKibana": true,
        "instanceCategory": "IS",
        "dedicateMaster": false,
        "advancedDedicateMaster": false,
        "masterConfiguration": {},
        "haveClientNode": false,
        "warmNode": true,
        "warmNodeConfiguration": {
            "spec": "elasticsearch.d1.2xlarge",
            "amount": 3
        },
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
        "enableKibanaPrivateNetwork": true,
        "advancedSetting": {
            "gcName": "CMS"
        },
        "enableMetrics": true,
        "readWritePolicy": {
            "writeHa": false
        }
    },
    "RequestId": "9E5466DE-E29A-4F87-9019-4AA7B80E60DE"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

