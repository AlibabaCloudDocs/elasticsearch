# UpdateDescription

Call UpdateDescription to update the name of a specified instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=UpdateDescription&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see Common parameters.

## Request syntax

```
PATCH|POST /openapi/instances/[InstanceId]/description HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1ptcb30009\*\*\*\*|The ID of the instance. |
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B350\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|description|String|Body|No|aliyunes\_name\_test|Specify the updated instance name. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|FDF34727-1664-44C1-A8DA-3EB72D60\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|description|String|aliyunes\_test\_name|The new name of the instance. |

**Note:** In the following return example, only the parameters in the returned data list are included in this article. The parameters that are not mentioned are for reference only. For more information about Parameter descriptions, see [ListInstance](~~142230~~) . It is not mandatory for a program to obtain these parameters.

## Examples

Sample requests

```
PATCH /openapi/instances/es-cn-n6w1ptcb30009****/description?description=aliyunes_name_test HTTP/1.1
Common request parameters
```

Sample success responses

`XML` format

```
<Result>
    <instanceId>es-cn-n6w1ptcb30009****</instanceId>
    <version>5.5.3_with_X-Pack</version>
    <description>aliyunes_name_test</description>
    <nodeAmount>3</nodeAmount>
    <paymentType>postpaid</paymentType>
    <status>activating</status>
    <privateNetworkIpWhiteList>0.0.0.0/0</privateNetworkIpWhiteList>
    <enablePublic>true</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.n4.small</spec>
        <disk>40</disk>
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
    <updatedAt>2020-07-06T09:21:11.615Z</updatedAt>
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
    <endTime>4749724800000</endTime>
    <clusterTasks>
        <type>updating</type>
        <progress>59.84375</progress>
        <status>RUNNING</status>
        <canCancelable>false</canCancelable>
        <interruptible>true</interruptible>
        <subTasks>
            <type>ecs</type>
            <progress>100</progress>
            <detail>
                <totalNodeCount>4</totalNodeCount>
                <completedNodeCount>4</completedNodeCount>
            </detail>
            <status>FINISHED</status>
            <canCancelable>false</canCancelable>
            <interruptible>false</interruptible>
        </subTasks>
        <subTasks>
            <type>hippo</type>
            <progress>100</progress>
            <detail/>
            <status>FINISHED</status>
            <canCancelable>false</canCancelable>
            <interruptible>false</interruptible>
        </subTasks>
        <subTasks>
            <type>rolling</type>
            <progress>39.375</progress>
            <detail>
                <totalNodeCount>4</totalNodeCount>
                <completedNodeCount>1</completedNodeCount>
            </detail>
            <status>RUNNING</status>
            <canCancelable>false</canCancelable>
            <interruptible>false</interruptible>
        </subTasks>
        <subTasks>
            <type>finally</type>
            <progress>0</progress>
            <detail/>
            <status>READY</status>
            <canCancelable>false</canCancelable>
            <interruptible>false</interruptible>
        </subTasks>
    </clusterTasks>
    <vpcInstanceId>es-cn-n6w1ptcb30009****-worker</vpcInstanceId>
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-hangzhou-i</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>elasticsearch</instanceType>
    <inited>true</inited>
    <domain>es-cn-n6w1ptcb30009****.elasticsearch.aliyuncs.com</domain>
    <port>9200</port>
    <esVersion>5.5.3_with_X-Pack</esVersion>
    <esConfig>
        <action.destructive_requires_name>true</action.destructive_requires_name>
        <xpack.security.audit.outputs>index</xpack.security.audit.outputs>
        <xpack.watcher.enabled>false</xpack.watcher.enabled>
        <xpack.security.audit.enabled>false</xpack.security.audit.enabled>
        <action.auto_create_index>true</action.auto_create_index>
    </esConfig>
    <esIPWhitelist>0.0.0.0/0</esIPWhitelist>
    <kibanaIPWhitelist>0.0.0.0/0</kibanaIPWhitelist>
    <kibanaIPWhitelist>::/0</kibanaIPWhitelist>
    <publicIpWhitelist>::1</publicIpWhitelist>
    <publicIpWhitelist>0.0.0.0/0</publicIpWhitelist>
    <kibanaDomain>es-cn-n6w1ptcb30009****.kibana.elasticsearch.aliyuncs.com</kibanaDomain>
    <kibanaPort>5601</kibanaPort>
    <publicPort>9200</publicPort>
    <publicDomain>es-cn-n6w1ptcb30009****.public.elasticsearch.aliyuncs.com</publicDomain>
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
    <dictList>
        <name>SYSTEM_MAIN.dic</name>
        <fileSize>3058510</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>MAIN</type>
    </dictList>
    <dictList>
        <name>SYSTEM_STOPWORD.dic</name>
        <fileSize>164</fileSize>
        <sourceType>ORIGIN</sourceType>
        <type>STOP</type>
    </dictList>
    <haveGrafana>false</haveGrafana>
    <haveCerebro>false</haveCerebro>
    <enableKibanaPublicNetwork>true</enableKibanaPublicNetwork>
    <enableKibanaPrivateNetwork>false</enableKibanaPrivateNetwork>
    <advancedSetting>
        <gcName>CMS
    </advancedSetting>
</Result>
<RequestId>24A68E4C-94B4-45E4-9068-CB12F33C****</RequestId>
```

`JSON` Syntax

```
{
    "Result": {
        "instanceId": "es-cn-n6w1ptcb30009****",
        "version": "5.5.3_with_X-Pack",
        "description": "aliyunes_name_test",
        "nodeAmount": 3,
        "paymentType": "postpaid",
        "status": "activating",
        "privateNetworkIpWhiteList": [
            "0.0.0.0/0"
        ],
        "enablePublic": true,
        "nodeSpec": {
            "spec": "elasticsearch.n4.small",
            "disk": 40,
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
        "updatedAt": "2020-07-06T09:21:11.615Z",
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
        "endTime": 4749724800000,
        "clusterTasks": [
            {
                "type": "updating",
                "progress": 59.84375,
                "status": "RUNNING",
                "canCancelable": false,
                "interruptible": true,
                "subTasks": [
                    {
                        "type": "ecs",
                        "progress": 100,
                        "detail": {
                            "totalNodeCount": 4,
                            "completedNodeCount": 4
                        },
                        "status": "FINISHED",
                        "canCancelable": false,
                        "interruptible": false,
                        "subTasks": []
                    },
                    {
                        "type": "hippo",
                        "progress": 100,
                        "detail": {},
                        "status": "FINISHED",
                        "canCancelable": false,
                        "interruptible": false,
                        "subTasks": []
                    },
                    {
                        "type": "rolling",
                        "progress": 39.375,
                        "detail": {
                            "totalNodeCount": 4,
                            "completedNodeCount": 1
                        },
                        "status": "RUNNING",
                        "canCancelable": false,
                        "interruptible": false,
                        "subTasks": []
                    },
                    {
                        "type": "finally",
                        "progress": 0,
                        "detail": {},
                        "status": "READY",
                        "canCancelable": false,
                        "interruptible": false,
                        "subTasks": []
                    }
                ]
            }
        ],
        "vpcInstanceId": "es-cn-n6w1ptcb30009****-worker",
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
        "domain": "es-cn-n6w1ptcb30009****.elasticsearch.aliyuncs.com",
        "port": 9200,
        "esVersion": "5.5.3_with_X-Pack",
        "esConfig": {
            "action.destructive_requires_name": "true",
            "xpack.security.audit.outputs": "index",
            "xpack.watcher.enabled": "false",
            "xpack.security.audit.enabled": "false",
            "action.auto_create_index": "true"
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
        "publicIpWhitelist": [
            "::1",
            "0.0.0.0/0"
        ],
        "kibanaDomain": "es-cn-n6w1ptcb30009****.kibana.elasticsearch.aliyuncs.com",
        "kibanaPort": 5601,
        "publicPort": 9200,
        "publicDomain": "es-cn-n6w1ptcb30009****.public.elasticsearch.aliyuncs.com",
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
                "fileSize": 3058510,
                "sourceType": "ORIGIN",
                "type": "MAIN"
            },
            {
                "name": "SYSTEM_STOPWORD.dic",
                "fileSize": 164,
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
    "RequestId": "24A68E4C-94B4-45E4-9068-CB12F33C****"
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|400|InstanceNotFound|The instanceId provided does not exist.|The error message returned because the instance cannot be found. Check the status of the instance.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

