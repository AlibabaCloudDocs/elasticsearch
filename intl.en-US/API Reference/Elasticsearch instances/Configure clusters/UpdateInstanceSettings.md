# UpdateInstanceSettings

Call UpdateInstanceSettings to update the YML configuration of a specified instance.

When you call this operation, take note of the following items:

When the instance is in the activating, invalid, or inactive state, you cannot update the configuration.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateInstanceSettings&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
PATCH|POST /openapi/instances/[InstanceId]/instance-settings HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-nif1q9o8r0008\*\*\*\*|The ID of the instance. |
|clientToken|String|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|A unique token generated by the client to guarantee the idempotency of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can only contain ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

The **esConfig** parameter needs to be filled in the RequestBody to specify the YML parameters and values to be updated. Example:

```

{
    "esConfig": {
        "thread_pool.bulk.queue_size": 500
    }
}
            
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BB1C321A-211C-4FD7-BD8B-7F2FABE2\*\*\*\*|The ID of the request. |

The returned data also includes the Result parameter. For more information, see [ListInstance](~~142230~~).

## Examples

Sample requests

```
PATCH /openapi/instances/es-cn-nif1q9o8r0008****/instance-settings HTTP/1.1
Common request parameters
{
    "esConfig": {
        "thread_pool.bulk.queue_size": 500
    }
}
```

Sample success responses

`JSON` format

```
{
    "Result": {
        "instanceId": "es-cn-nif1q9o8r0008****",
        "version": "6.7.0_with_X-Pack",
        "description": "es-cn-nif1q9o8r0008****",
        "nodeAmount": 4,
        "paymentType": "postpaid",
        "status": "active",
        "privateNetworkIpWhiteList": [
            "0.0.0.0/0"
        ],
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
        "createdAt": "2020-07-07T04:05:16.791Z",
        "updatedAt": "2020-07-07T07:09:51.268Z",
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
                "aliVersion": "ali1.2.0"
            }
        ],
        "endTime": 4749811200000,
        "clusterTasks": [
            {
                "type": "MigrateData",
                "progress": 100,
                "detail": {},
                "status": "FINISHED",
                "canCancelable": false,
                "interruptible": false,
                "subTasks": [
                    {
                        "type": "FindShrinkNodeAction",
                        "progress": 100,
                        "detail": {},
                        "status": "FINISHED",
                        "canCancelable": false,
                        "interruptible": false,
                        "subTasks": []
                    },
                    {
                        "type": "MigrateDataAction",
                        "progress": 100,
                        "detail": {
                            "doneMigrateNodeIps": [
                                "172.16. **.**"
                            ],
                            "allMigrateNodeIps": [
                                "172.16. **.**"
                            ]
                        },
                        "status": "FINISHED",
                        "canCancelable": false,
                        "interruptible": false,
                        "subTasks": []
                    }
                ]
            }
        ],
        "vpcInstanceId": "es-cn-nif1q9o8r0008****-worker",
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
        "domain": "es-cn-nif1q9o8r0008****.elasticsearch.aliyuncs.com",
        "port": 9200,
        "esVersion": "6.7.0_with_X-Pack",
        "esConfig": {
            "thread_pool.bulk.queue_size": "500",
            "xpack.security.authc.realms.native.type": "native",
            "xpack.security.authc.reserved_realm.enabled": "false",
            "xpack.security.transport.ssl.truststore.path": "168520994880****.p12",
            "xpack.security.authc.realms.native.order": "1",
            "xpack.license.self_generated.type": "trial",
            "xpack.security.authc.realms.file.order": "0",
            "xpack.security.authc.realms.file.type": "file",
            "xpack.security.enabled": "true",
            "bootstrap.memory_lock": "true",
            "xpack.monitoring.collection.enabled": "true",
            "xpack.security.transport.ssl.keystore.path": "168520994880****.p12",
            "xpack.security.transport.ssl.verification_mode": "certificate",
            "xpack.security.transport.ssl.enabled": "true"
        },
        "esIPWhitelist": [
            "0.0.0.0/0"
        ],
        "esIPBlacklist": [],
        "kibanaIPWhitelist": [
            "0.0.0.0/0",
            "::/0"
        ],
        "kibanaPrivateIPWhitelist": [],
        "publicIpWhitelist": [],
        "kibanaDomain": "es-cn-nif1q9o8r0008****.kibana.elasticsearch.aliyuncs.com",
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
        }
    },
    "RequestId": "C1FA70F8-B84E-4D89-B31A-BCD1E476****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the specified instance cannot be found. Check the instance status.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

