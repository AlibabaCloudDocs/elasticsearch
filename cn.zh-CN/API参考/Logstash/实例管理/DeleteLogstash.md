# DeleteLogstash

调用DeleteLogstash，释放指定按量付费的阿里云Logstash实例。

在调用接口前，请注意：

释放后，实例所使用的物理资源都被回收，相关数据全部丢失且不可恢复；挂载实例节点的云盘也会被释放；相应的快照会被删除。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DeleteLogstash&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
DELETE /openapi/logstashes/[InstanceId] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-n6w1o5jq\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|deleteType|String|Query|否|protective|释放类型，可选值：

 -   immediate：立即删除。删除后，系统会彻底清除所有数据，且实例不再显示在实例列表中。
-   protective：实例会被冻结24小时后，再彻底清除数据，期间实例仍在实例列表中显示，您可以选择[恢复实例](~~202205~~)或[立即释放](~~160591~~)。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|94B03BBA-A132-42C3-8367-0A0C1C45\*\*\*\*|请求ID。 |

返回数据中还包括Result参数，参数说明请参见[ListLogstash](~~160534~~)。

## 示例

请求示例

```
DELETE /openapi/logstashes/ls-cn-n6w1o5jq****?deleteType=protective HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <instanceId>ls-cn-m7r1vsi2****</instanceId>
    <version>7.4.0_with_X-Pack</version>
    <description>ls-cn-m7r1vsi2****</description>
    <nodeAmount>1</nodeAmount>
    <paymentType>postpaid</paymentType>
    <status>inactive</status>
    <enablePublic>false</enablePublic>
    <nodeSpec>
        <spec>elasticsearch.sn1ne.large</spec>
        <disk>20</disk>
        <diskType>cloud_ssd</diskType>
    </nodeSpec>
    <networkConfig>
        <vpcId>vpc-uf6gbyjqof623050q****</vpcId>
        <vswitchId>vsw-uf6thgrj0ei9zmo9n****</vswitchId>
        <vsArea>cn-shanghai-g</vsArea>
        <type>vpc</type>
    </networkConfig>
    <createdAt>2020-10-22T12:54:34.485Z</createdAt>
    <updatedAt>2021-02-01T02:59:39.177Z</updatedAt>
    <commodityCode>elasticsearch_logstash_post</commodityCode>
    <extendConfigs>
        <configType>deleteProtection</configType>
        <deleteTime>1612234779084</deleteTime>
    </extendConfigs>
    <endTime>4759056000000</endTime>
    <resourceGroupId>rg-acfm2h5vbzd****</resourceGroupId>
    <zoneCount>1</zoneCount>
    <protocol>HTTP</protocol>
    <zoneInfos>
        <zoneId>cn-shanghai-g</zoneId>
        <status>NORMAL</status>
    </zoneInfos>
    <instanceType>logstash</instanceType>
    <inited>true</inited>
    <tags>
        <tagKey>acs:rm:rgId</tagKey>
        <tagValue>rg-acfm2h5vbzd****</tagValue>
    </tags>
    <config>
        <slowlog.threshold.warn>2s</slowlog.threshold.warn>
        <slowlog.threshold.info>1s</slowlog.threshold.info>
        <slowlog.threshold.debug>500ms</slowlog.threshold.debug>
        <slowlog.threshold.trace>100ms</slowlog.threshold.trace>
    </config>
    <endpointList>
        <host>172.16.xx.xx</host>
        <port>9600</port>
        <zoneId>cn-shanghai-g</zoneId>
    </endpointList>
</Result>
<RequestId>9E3C8216-DDDE-4EE3-81DF-168135E074E1</RequestId>
```

`JSON`格式

```
{
  "Result": {
    "instanceId": "ls-cn-m7r1vsi2****",
    "version": "7.4.0_with_X-Pack",
    "description": "ls-cn-m7r1vsi2****",
    "nodeAmount": 1,
    "paymentType": "postpaid",
    "status": "inactive",
    "enablePublic": false,
    "nodeSpec": {
      "spec": "elasticsearch.sn1ne.large",
      "disk": 20,
      "diskType": "cloud_ssd"
    },
    "networkConfig": {
      "vpcId": "vpc-uf6gbyjqof623050q****",
      "vswitchId": "vsw-uf6thgrj0ei9zmo9n****",
      "vsArea": "cn-shanghai-g",
      "type": "vpc"
    },
    "createdAt": "2020-10-22T12:54:34.485Z",
    "updatedAt": "2021-02-01T02:59:39.177Z",
    "commodityCode": "elasticsearch_logstash_post",
    "extendConfigs": [
      {
        "configType": "deleteProtection",
        "deleteTime": 1612234779084
      }
    ],
    "endTime": 4759056000000,
    "clusterTasks": [],
    "resourceGroupId": "rg-acfm2h5vbzd****",
    "zoneCount": 1,
    "protocol": "HTTP",
    "zoneInfos": [
      {
        "zoneId": "cn-shanghai-g",
        "status": "NORMAL"
      }
    ],
    "instanceType": "logstash",
    "inited": true,
    "tags": [
      {
        "tagKey": "acs:rm:rgId",
        "tagValue": "rg-acfm2h5vbzd****"
      }
    ],
    "config": {
      "slowlog.threshold.warn": "2s",
      "slowlog.threshold.info": "1s",
      "slowlog.threshold.debug": "500ms",
      "slowlog.threshold.trace": "100ms"
    },
    "endpointList": [
      {
        "host": "172.16.xx.xx",
        "port": 9600,
        "zoneId": "cn-shanghai-g"
      }
    ]
  },
  "RequestId": "9E3C8216-DDDE-4EE3-81DF-168135E074E1"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

