# GetRegionConfiguration

获取当前区域的开放配置信息。接口返回值供参考，以控制台和售卖页实际展示值为准。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=GetRegionConfiguration&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/region HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|zoneId|String|Query|否|cn-hangzhou|当前区域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6F\*\*\*\*\*\*|请求ID。 |
|Result|Struct| |返回的区域配置信息。 |
|clientNodeAmountRange|Struct| |协调节点节点数范围。 |
|maxAmount|Integer|25|协调节点节点数最大值。 |
|minAmount|Integer|2|协调节点节点数最小值。 |
|clientNodeDiskList|Array of disk| |协调节点磁盘允许值。 |
|diskType|String|cloud\_efficiency|磁盘存储类型。 |
|maxSize|Integer|20|磁盘允许最大值。 |
|minSize|Integer|20|磁盘允许最小值。 |
|scaleLimit|Integer|18|磁盘允许设置连续值的最大值。 |
|clientNodeSpec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.4xlarge","elasticsearch.ic5.xlarge","elasticsearch.ic5.2xlarge","elasticsearch.ic5.3xlarge","elasticsearch.ic5.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge"\]|协调节点规格。 |
|createUrl|String|https://common-buy.aliyun.com/?commodityCode=elasticsearch&orderType=BUY\#/buy|售卖页入口地址。 |
|dataDiskList|Array of dataDiskList| |数据节点磁盘允许值。 |
|diskType|String|cloud\_ssd|磁盘存储类型。 |
|maxSize|Integer|5120|磁盘允许最大值。 |
|minSize|Integer|20|磁盘允许最小值。 |
|scaleLimit|Integer|2048|磁盘允许设置连续值的最大值。 |
|valueLimitSet|List|\[2560,3072,3584,4096,4608,5120\]|磁盘允许的离散值。 |
|elasticNodeProperties|Struct| |弹性节点配置。 |
|amountRange|Struct| |冷节点节点数范围值。 |
|maxAmount|Integer|25|节点数最大值。 |
|minAmount|Integer|2|节点数最小值。 |
|diskList|Array of disk| |磁盘配置列表。 |
|diskEncryption|Boolean|true|是否允许磁盘加密。取值含义如下：

 -   true：允许加密。
-   flase：不允许加密。 |
|diskType|String|cloud\_ssd|磁盘存储类型。 |
|maxSize|Integer|5120|磁盘允许最大值。 |
|minSize|Integer|500|磁盘允许最小值。 |
|scaleLimit|Integer|2048|磁盘允许设置连续值的最大值。 |
|valueLimitSet|List|\[2560,3072,3584,4096,4608,5120\]|磁盘允许的离散值。 |
|spec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.8xlarge","elasticsearch.ic5.large","elasticsearch.ic5.xlarge","elasticsearch.ic5.2xlarge","elasticsearch.ic5.3xlarge","elasticsearch.ic5.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge"\]|节点规格。 |
|env|String|production|环境标志。 |
|esVersions|List|\[ "5.5.3\_with\_X-Pack"\]|允许创建的Elasticsearch版本列表。 |
|esVersionsLatestList|Array of esVersionsLatestList| |ES版本开放售卖列表。 |
|key|String|5.5\_with\_X-Pack|支持的大版本号。 |
|value|String|5.5.3\_with\_X-Pack|支持的小版本号全称 。 |
|instanceSupportNodes|List|\[ "WORKER", "WORKER\_WARM", "COORDINATING", "KIBANA", "MASTER", "ELASTIC\_WORKER" \]|区域开放的实例节点类型。 |
|jvmConfine|Struct| |Jvm校验配置。 |
|memory|Integer|32|开启Jvm回收所需规格的内存最小值。 |
|supportEsVersions|List|\["6.7.0\_with\_X-Pack","6.7.0\_with\_A-Pack","7.4.0\_with\_X-Pack"\]|开启Jvm回收支持的ES版本信息。 |
|supportGcs|List|\["CMS","G1"\]|允许设置的Jvm回收器列表。 |
|kibanaNodeProperties|Struct| |Kibana节点配置。 |
|amountRange|Struct| |节点数允许值范围。 |
|maxAmount|Integer|20|节点数最大值。 |
|minAmount|Integer|1|节点数最小值。 |
|spec|List|\["elasticsearch.n4.small","elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn1ne.large"\]|允许设置的规格列表。 |
|masterDiskList|Array of disk| |专有主节点磁盘允许值。 |
|diskType|String|cloud\_ssd|磁盘存储类型。 |
|maxSize|Integer|20|磁盘允许最大值。 |
|minSize|Integer|20|磁盘允许最小值。 |
|scaleLimit|Integer|20|磁盘允许设置连续值的最大值。 |
|masterSpec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge" \]|专有主节点规格。 |
|node|Struct| |节点配置。 |
|maxAmount|Integer|50|数据节点允许的最大节点数。 |
|minAmount|Integer|2|数据节点允许的最小节点数。 |
|nodeSpecList|Array of nodeSpecList| |数据节点规格列表。 |
|cpuCount|Integer|16|该规格对应的CPU核数。 |
|disk|Integer|44000|该规格对应的磁盘大小。 |
|diskType|String|local\_efficiency|磁盘存储类型。 |
|enable|Boolean|true|是否可以购买：true（可以购买）、flase（不可以购买）。 |
|memorySize|Integer|64|节点内存大小。 |
|spec|String|elasticsearch.sn2ne.large|规格名称。 |
|specGroupType|String|local\_efficiency|存储类型，支持以下三种类型：

 -   common：云盘型
-   local\_efficiency：本地SATA盘。
-   local\_ssd：本地SSD盘。 |
|regionId|String|cn-hangzhou|当前区域 ID。 |
|supportVersions|Array of CategoryEntity| |支持的版本配置。 |
|instanceCategory|String|x-pack|实例类别，支持以下两种类别：

 -   advanced增强版。
-   x-pack商业版。 |
|supportVersionList|Array of VersionEntity| |支持的Elsticsearch版本信息。 |
|key|String|5.5|售卖页支持可选的版本。 |
|value|String|5.5.3|详细版本号。 |
|warmNodeProperties|Struct| |冷节点配置。 |
|amountRange|Struct| |节点数范围值。 |
|maxAmount|Integer|50|节点数最大值。 |
|minAmount|Integer|2|节点数最小值。 |
|diskList|Array of disk| |磁盘配置列表。 |
|diskEncryption|Boolean|true|磁盘允许加密。 |
|diskType|String|cloud\_efficiency|磁盘存储类型。 |
|maxSize|Integer|5120|磁盘允许最大值。 |
|minSize|Integer|500|磁盘允许最小值。 |
|scaleLimit|Integer|2048|磁盘允许设置连续值的最大值。 |
|valueLimitSet|List|\[2560,3072,3584,4096,4608,5120\]|磁盘允许的离散值。 |
|spec|List|\["elasticsearch.sn2ne.large","elasticsearch.sn2ne.xlarge","elasticsearch.sn2ne.2xlarge","elasticsearch.sn2ne.4xlarge","elasticsearch.sn1ne.8xlarge","elasticsearch.ic5.large","elasticsearch.ic5.xlarge","elasticsearch.ic5.2xlarge","elasticsearch.ic5.3xlarge","elasticsearch.ic5.4xlarge","elasticsearch.r5.large","elasticsearch.r5.xlarge","elasticsearch.r5.2xlarge"\]|冷节点支持的规格。 |
|zones|List|\["cn-hangzhou-b","cn-hangzhou-f"\]|支持的可用区。 |

**说明：** 以上的示例值仅供参考，以接口实际返回值为准。

## 示例

请求示例

```
GET /openapi/region
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6F******",
    "Result": {
        "regionId": "cn-hangzhou",
        "createUrl": "https://common-buy.aliyun.com/?commodityCode=elasticsearch&amp;orderType=BUY#/buy",
        "env": "production",
        "dataDiskList": {
            "scaleLimit": 2048,
            "maxSize": 5120,
            "minSize": 20,
            "diskType": "cloud_ssd",
            "valueLimitSet": "[2560,3072,3584,4096,4608,5120]"
        },
        "supportVersions": {
            "instanceCategory": "x-pack",
            "supportVersionList": {
                "value": "5.5.3",
                "key": 5.5
            }
        },
        "nodeSpecList": {
            "disk": 44000,
            "memorySize": 64,
            "enable": true,
            "diskType": "local_efficiency",
            "specGroupType": "local_efficiency",
            "spec": "elasticsearch.sn2ne.large",
            "cpuCount": 16
        },
        "clientNodeDiskList": {
            "scaleLimit": 18,
            "maxSize": 20,
            "minSize": 20,
            "diskType": "cloud_efficiency"
        },
        "esVersionsLatestList": {
            "value": "5.5.3_with_X-Pack",
            "key": "5.5_with_X-Pack"
        },
        "masterDiskList": {
            "scaleLimit": 20,
            "maxSize": 20,
            "minSize": 20,
            "diskType": "cloud_ssd"
        },
        "zones": "[\"cn-hangzhou-b\",\"cn-hangzhou-f\"]",
        "esVersions": "[ \"5.5.3_with_X-Pack\"]",
        "masterSpec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\" ]",
        "clientNodeSpec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.4xlarge\",\"elasticsearch.ic5.xlarge\",\"elasticsearch.ic5.2xlarge\",\"elasticsearch.ic5.3xlarge\",\"elasticsearch.ic5.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\"]",
        "instanceSupportNodes": "[ \"WORKER\", \"WORKER_WARM\", \"COORDINATING\", \"KIBANA\", \"MASTER\", \"ELASTIC_WORKER\" ]",
        "node": {
            "minAmount": 2,
            "maxAmount": 50
        },
        "jvmConfine": {
            "memory": 32,
            "supportGcs": "[\"CMS\",\"G1\"]",
            "supportEsVersions": "[\"6.7.0_with_X-Pack\",\"6.7.0_with_A-Pack\",\"7.4.0_with_X-Pack\"]"
        },
        "clientNodeAmountRange": {
            "minAmount": 2,
            "maxAmount": 25
        },
        "warmNodeProperties": {
            "diskList": {
                "scaleLimit": 2048,
                "minSize": 500,
                "maxSize": 5120,
                "diskType": "cloud_efficiency",
                "diskEncryption": true,
                "valueLimitSet": "[2560,3072,3584,4096,4608,5120]"
            },
            "spec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.8xlarge\",\"elasticsearch.ic5.large\",\"elasticsearch.ic5.xlarge\",\"elasticsearch.ic5.2xlarge\",\"elasticsearch.ic5.3xlarge\",\"elasticsearch.ic5.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\"]",
            "amountRange": {
                "minAmount": 2,
                "maxAmount": 50
            }
        },
        "kibanaNodeProperties": {
            "spec": "[\"elasticsearch.n4.small\",\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn1ne.large\"]",
            "amountRange": {
                "minAmount": 1,
                "maxAmount": 20
            }
        },
        "elasticNodeProperties": {
            "diskList": {
                "scaleLimit": 2048,
                "minSize": 500,
                "maxSize": 5120,
                "diskType": "cloud_ssd",
                "diskEncryption": true,
                "valueLimitSet": "[2560,3072,3584,4096,4608,5120]"
            },
            "spec": "[\"elasticsearch.sn2ne.large\",\"elasticsearch.sn2ne.xlarge\",\"elasticsearch.sn2ne.2xlarge\",\"elasticsearch.sn2ne.4xlarge\",\"elasticsearch.sn1ne.8xlarge\",\"elasticsearch.ic5.large\",\"elasticsearch.ic5.xlarge\",\"elasticsearch.ic5.2xlarge\",\"elasticsearch.ic5.3xlarge\",\"elasticsearch.ic5.4xlarge\",\"elasticsearch.r5.large\",\"elasticsearch.r5.xlarge\",\"elasticsearch.r5.2xlarge\"]",
            "amountRange": {
                "minAmount": 2,
                "maxAmount": 25
            }
        }
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

