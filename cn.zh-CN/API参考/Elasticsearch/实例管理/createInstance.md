# createInstance

调用createInstance，创建Elasticsearch实例。

在调用接口前，请注意：

-   请确保在使用该接口前，已充分了解Elasticsearch产品的收费方式和价格。

    详情请参见[阿里云Elasticsearch定价](https://www.aliyun.com/price/product?spm=a2c4g.11186623.2.7.657d2cbeRoSPCd#/elasticsearch/detail)。

-   创建实例需要通过实名认证。

    详情请参见[实名认证](~~37175~~)。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=createInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需要填入以下参数，用来指定待创建的实例信息。

|参数

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|paymentType

|String

|是

|postpaid

|付费类型。可选值：postpaid（按量计费）、prepaid（包年包月）。 |
|paymentInfo

|Map

|否

|postpaid

|包年包月实例的付费详情。 |
|└duration

|Integer

|否

|1

|购买时间。支持按月和按年购买。 |
|└pricingCycle

|String

|否

|Month

|包年包月单位，支持：Year（年）、Month（月）。 |
|└isAutoRenew

|Boolean

|否

|true

|是否开启自动续费设置，支持：true（开启）、false（不开启）。 |
|└autoRenewDuration

|Integer

|否

|3

|自动续费周期，单位：月。 |
|nodeAmount

|int

|是

|3

|数据节点个数。 |
|instanceCategory

|String

|否

|advanced

|版本类型。支持advanced（增强版）、x-pack（商业版）。设置为advanced时，必须购买Master节点和CPFS共享存储。 |
|esAdminPassword

|String

|是

|es\_password

|ES实例的访问密码。要求包含以下字符中的三种：大写字母、小写字母、数字、特殊字符：!@\#$%^&\*\(\)\_+-=，长度为8~32位。 |
|esVersion

|String

|是

|5.5.3\_with\_X-Pack

|实例版本。可选值：7.10\_with\_X-Pack、6.7\_with\_X-Pack、6.7\_with\_A-Pack、7.7\_with\_X-Pack、6.8\_with\_X-Pack、6.3\_with\_X-Pack、5.6\_with\_X-Pack、5.5.3\_with\_X-Pack。 |
|nodeSpec

|Map

|是

| |数据节点配置。 |
|└spec

|String

|是

|elasticsearch.sn2ne.xlarge

|节点规格，规格信息可通过[产品规格](~~271718~~)查看。 |
|└disk

|String

|是

|20

|单节点存储空间，单位：GB。 |
|└diskType

|String

|是

|cloud\_ssd

|存储类型。可选值：cloud\_ssd（SSD云盘）、cloud\_essd（ESSD云盘）、cloud\_efficiency（高效云盘）。 |
|└performanceLevel

|String

|否

|PL1

|ESSD云盘的性能级别。当存储类型为cloud\_essd时，该参数必选，支持PL1、PL2、PL3。 |
|└diskEncryption

|Boolean

|否

|true

|是否开启云盘加密，支持：true（开启）、false（不开启）。 |
|advancedDedicateMaster

|boolean

|否

|false

|是否创建专有主节点，如果是多可用区部署则必选。 |
|masterConfiguration

|Map

|否

| |专有主节点配置。advancedDedicateMaster为true时必填。 |
|└spec

|String

|是

|elasticsearch.sn2ne.xlarge

|节点规格，规格信息可通过[产品规格](~~271718~~)查看。 |
|└amount

|int

|是

|3

|节点数量，目前固定为3。 |
|└disk

|int

|是

|20

|单节点存储空间大小。当前只支持20GB。 |
|└diskType

|string

|是

|cloud\_ssd

|节点存储类型。可选值：cloud\_ssd（SSD云盘）、cloud\_essd（ESSD云盘）。 |
|warmNode

|boolean

|否

|false

|是否购买冷数据节点。 |
|warmNodeConfiguration

|Map

|否

| |冷数据节点配置。warmNode为true时必填。 |
|└spec

|string

|是

|elasticsearch.ic5.large

|节点规格，规格信息可通过[产品规格](~~271718~~)查看。 |
|└amount

|Integer

|是

|2

|节点数量。 |
|└diskType

|string

|是

|cloud\_efficiency

|节点存储类型。可选值：cloud\_efficiency（高效云盘）。 |
|└disk

|Integer

|是

|500

|单节点存储空间。 |
|└diskEncryption

|Boolean

|否

|true

|是否开启云盘加密，支持：true（开启）、false（不开启）。 |
|haveClientNode

|boolean

|否

|false

|是否购买协调节点。 |
|clientNodeConfiguration

|Map

|否

| |协调节点配置。haveClientNode为true时必填。 |
|└spec

|string

|是

|elasticsearch.ic5.large

|节点规格，规格信息可通过[产品规格](~~271718~~)查看。 |
|└amount

|Integer

|是

|2

|节点数量。 |
|└diskType

|string

|是

|cloud\_efficiency

|节点存储类型。可选值：cloud\_efficiency（高效云盘）。 |
|└disk

|Integer

|是

|20

|单节点存储空间大小。 |
|haveElasticDataNode

|boolean

|否

|false

|是否购买弹性节点。购买弹性节点前，需要先购买专有主节点。 |
|elasticDataNodeConfiguration

|Map

|否

| |弹性节点配置。haveElasticDataNode为true时必填。 |
|└spec

|string

|是

|elasticsearch.ic5.large

|节点规格，规格信息可通过[产品规格](~~271718~~)查看。 |
|└amount

|Integer

|是

|2

|数量。 |
|└diskType

|string

|是

|cloud\_efficiency

|节点存储类型。可选值：cloud\_ssd（SSD云盘）、cloud\_essd（ESSD云盘）、cloud\_efficiency（高效云盘）。 |
|└disk

|Integer

|是

|20

|单节点存储空间大小。 |
|└performanceLevel

|String

|否

|PL1

|ESSD云盘的性能级别。当存储类型为cloud\_essd时，该参数必选，支持PL1、PL2、PL3。 |
|└diskEncryption

|Boolean

|否

|true

|是否开启云盘加密，支持：true（开启）、false（不开启）。 |
|haveKibana

|boolean

|否

|true

|是否购买kibana节点。 |
|kibanaConfiguration

|Map

|否

| |kibana节点配置。haveKibana为true时必填。 |
|└spec

|String

|是

|elasticsearch.n4.small

|节点规格，规格信息可通过[产品规格](~~271718~~)查看。 |
|└amount

|Integer

|是

|1

|节点数量，目前固定为1。 |
|└disk

|Integer

|是

|0

|存储大小，目前固定为0。 |
|networkConfig

|Map

|是

| |网络配置。 |
|└type

|string

|是

|VPC

|网络类型。目前仅支持专有网络。 |
|└vpcId

|string

|是

|vpc-bp16k1dvzxtmagcva\*\*\*\*

|专有网络ID。 |
|└vsArea

|string

|是

|cn-hangzhou-i

|交换机所在的可用区。 |
|└vswitchId

|string

|是

|vsw-bp1k4ec6s7sjdbudw\*\*\*\*

|交换机ID。 |
|extendConfigs

|list

|否

| |实例扩展配置。 |
|└configType

|string

|是

|sharedDisk

|配置类型，固定为sharedDisk（共享存储），仅适用于增强版实例。 |
|└disk

|Integer

|是

|5120

|共享存储磁盘大小。 |
|dryRun

|boolean

|否

|true

|创建实例时是否校验配置，可选值：true（只校验，不创建）、false（校验并创建）。 |

**说明：**

-   └表示子参数。
-   支持的节点规格列表，请参见[阿里云Elasticsearch定价信息](https://www.aliyun.com/price/product?spm=a2c4g.11186623.2.10.653c6c88NcQPZY#/elasticsearch/detail)、[产品规格](~~271718~~)。

示例如下。

```

{
    "paymentType": "postpaid",
    "nodeAmount": "3",
    "instanceCategory": "x-pack",
    "esAdminPassword": "es_password",
    "esVersion": "6.7_with_X-Pack",
    "nodeSpec": {
        "spec": "elasticsearch.sn2ne.xlarge",
        "disk": "20",
        "diskType": "cloud_ssd"     
    },
    "networkConfig": {
        "type": "vpc",
        "vpcId": "vpc-bp16k1dvzxtmagcva****",
        "vsArea": "cn-hangzhou-i",
        "vswitchId": "vsw-bp1k4ec6s7sjdbudw****"
    }
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|838D9D11-8EEF-46D8-BF0D-BC8FC2B0C2F3|请求ID。 |
|Result|Struct| |返回结果。 |
|instanceId|String|es-cn-t57p81n7ai89v\*\*\*\*|实例ID。 |

## 示例

请求示例

```
POST /openapi/instances HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"instanceId": "es-cn-t57p81n7ai89v****"
	},
	"RequestId": "838D9D11-8EEF-46D8-BF0D-BC8FC2B0****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

