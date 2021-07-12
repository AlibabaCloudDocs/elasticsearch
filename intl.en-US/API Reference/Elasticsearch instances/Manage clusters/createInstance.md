# createInstance

You can call this operation to create an Elasticsearch instance.

Before you call the API operation, note that:

-   Make sure that you have read and understand the billing and pricing standards of Alibaba Cloud Elasticsearch.

    For more information, see [Alibaba Cloud Elasticsearch pricing](https://www.aliyun.com/price/product?spm=a2c4g.11186623.2.7.657d2cbeRoSPCd#/elasticsearch/detail).

-   Before you create an instance, you must complete real-name verification.

    .


## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=createInstance&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request headers. For more information, see Common request parameters.

## Request syntax

```

     POST /openapi/instances HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

You also need to specify the following parameters in RequestBody to specify the information of the instance to be created.

|Field

|Type

|Required

|Example

|Description |
|-------|------|----------|---------|-------------|
|paymentType

|String

|Yes

|postpaid

|The billing method of the cluster. Valid values: postpaid \(pay-as-you-go\) and prepaid \(subscription\). |
|paymentInfo

|Map

|No

|postpaid

|The payment details of a subscription instance. |
|└duration

|Integer

|No

|1

|The time of purchase. Subscriptions can be purchased on a monthly or yearly basis. |
|└pricingCycle

|String

|No

|Month

|The subscription unit. Valid values: Year and Month. |
|└isAutoRenew

|Boolean

|No

|true

|Specifies whether to enable the auto-renewal setting. Valid values: true \(enabled\) and false \(disabled\). |
|└autoRenewDuration

|Integer

|No

|3

|The auto-renewal period. Unit: month. |
|nodeAmount

|int

|Yes

|3

|The number of data nodes. |
|instanceCategory

|String

|No

|advanced

|Version type. Supported advanced \(enhanced edition\) and x-pack \(commercial edition\). If you set this parameter to advanced, you must purchase Master nodes and CPFS shared storage. |
|esAdminPassword

|String

|Yes

|es\_password

|The access password of the Elasticsearch instance. It is required to contain three of the following characters: uppercase letters, lowercase letters, digits, and special characters:!@\#$%^&\*\(\)\_+-=, 8 to 32 bits in length. |
|esVersion

|String

|Yes

|5.5.3\_with\_X-Pack

|The version of the instance. Valid values: 7.10\_with\_X-Pack, 6.7\_with\_X-Pack, 6.7\_with\_A-Pack, 7.7\_with\_X-Pack, 6.8\_with\_X-Pack, 6.3\_with\_X-Pack, 5.6\_with\_X-Pack, and 5.5.3\_with\_X-Pack. |
|nodeSpec

|Map

|Yes

| |Data node configurations. |
|└spec

|String

|Yes

|elasticsearch.sn2ne.xlarge

|Node type . |
|└disk

|String

|Yes

|20

|The storage space of a single node. Unit: GB. |
|└diskType

|String

|Yes

|cloud\_ssd

|The type of the disk. Valid values: cloud\_ssd\(SSD\), cloud\_essd \(Enhanced SSD\), and cloud\_efficiency \(ultra disk\). |
|└performanceLevel

|String

|No

|PL1

|The performance level of the ESSD. This parameter is required when the storage type is cloud\_essd. Valid values: PL1, PL2, and PL3. |
|└diskEncryption

|Boolean

|No

|true

|Specifies whether to enable disk encryption. Valid values: true \(enabled\) and false \(disabled\). |
|advancedDedicateMaster

|boolean

|No

|false

|Specifies whether to create a dedicated master node. This parameter is required if it is deployed in multiple zones. |
|masterConfiguration

|Map

|No

| |The configuration of the dedicated master node. This parameter is required if the advancedDedicateMaster is set to true. |
|└spec

|String

|Yes

|elasticsearch.sn2ne.xlarge

|Node type . |
|└amount

|int

|Yes

|3

|The number of nodes. Currently, the value is fixed to 3. |
|└disk

|int

|Yes

|20

|The size of the storage space per node. Currently, only 20GB is supported. |
|└diskType

|string

|Yes

|cloud\_ssd

|The storage type of the node. Valid values: cloud\_ssd\(SSD\) and cloud\_essd \(Enhanced SSD\). |
|warmNode

|boolean

|No

|false

|Specifies whether to purchase cold data nodes. |
|warmNodeConfiguration

|Map

|No

| |Cold data node configurations. This parameter is required if the warmNode parameter is set to true. |
|└spec

|string

|Yes

|elasticsearch.ic5.large

|Node type . |
|└amount

|Integer

|Yes

|2

|The number of nodes. |
|└diskType

|string

|Yes

|cloud\_efficiency

|The storage type of the node. Optional value: cloud\_efficiency \(Ultra Disk\). |
|└disk

|Integer

|Yes

|500

|Single-point storage space. |
|└diskEncryption

|Boolean

|No

|true

|Specifies whether to enable disk encryption. Valid values: true \(enabled\) and false \(disabled\). |
|haveClientNode

|boolean

|No

|false

|Specifies whether to purchase a coordination node. |
|clientNodeConfiguration

|Map

|No

| |Coordinate node configurations. This parameter is required when the haveClientNode parameter is set to true. |
|└spec

|string

|Yes

|elasticsearch.ic5.large

|Node type . |
|└amount

|Integer

|Yes

|2

|The number of nodes. |
|└diskType

|string

|Yes

|cloud\_efficiency

|The storage type of the node. Optional value: cloud\_efficiency \(Ultra Disk\). |
|└disk

|Integer

|Yes

|20

|The size of the storage space per node. |
|haveElasticDataNode

|boolean

|No

|false

|Specifies whether to purchase elastic nodes. Before you purchase elastic nodes, you need to purchase dedicated master nodes. |
|elasticDataNodeConfiguration

|Map

|No

| |The configuration of the elastic node. This parameter is required if the haveElasticDataNode is set to true. |
|└spec

|string

|Yes

|elasticsearch.ic5.large

|Node type . |
|└amount

|Integer

|Yes

|2

|The number of risk items. |
|└diskType

|string

|Yes

|cloud\_efficiency

|The storage type of the node. Valid values: cloud\_ssd\(SSD\), cloud\_essd \(Enhanced SSD\), and cloud\_efficiency \(ultra disk\). |
|└disk

|Integer

|Yes

|20

|The size of the storage space per node. |
|└performanceLevel

|String

|No

|PL1

|The performance level of the ESSD. This parameter is required when the storage type is cloud\_essd. Valid values: PL1, PL2, and PL3. |
|└diskEncryption

|Boolean

|No

|true

|Specifies whether to enable disk encryption. Valid values: true \(enabled\) and false \(disabled\). |
|haveKibana

|boolean

|No

|true

|Specifies whether to purchase kibana nodes. |
|kibanaConfiguration

|Map

|No

| |The configuration of the kibana node. This parameter is required when the haveKibana parameter is set to true. |
|└spec

|String

|Yes

|elasticsearch.n4.small

|Node type . |
|└amount

|Integer

|Yes

|1

|The number of nodes. Currently, the value is fixed to 1. |
|└disk

|Integer

|Yes

|0

|The storage size. Currently, the value is fixed to 0. |
|networkConfig

|Map

|Yes

| |The network configurations. |
|└type

|string

|Yes

|VPC

|The type of the network. Only VPC is supported. |
|└vpcId

|string

|Yes

|vpc-bp16k1dvzxtmagcva\*\*\*\*

|The ID of the VPC. |
|└vsArea

|string

|Yes

|cn-hangzhou-i

|The ID of the zone to which the VSwitch belongs. |
|└vswitchId

|string

|Yes

|vsw-bp1k4ec6s7sjdbudw\*\*\*\*

|The IDs of vSwitches. |
|extendConfigs

|list

|No

| |The extended configurations of the instance. |
|└configType

|string

|Yes

|sharedDisk

|The type of the configuration. Set this parameter to sharedDisk \(shared storage\). This parameter is applicable only to instances of the Enhanced Edition. |
|└disk

|Integer

|Yes

|5120

|The size of the shared storage disk. |
|dryRun

|boolean

|No

|true

|Specifies whether to verify the configuration when the instance is created. Valid values: true \(verify only, not create\) and false \(verify and create\). |

**Note:**

-   └ indicates a child parameter.
-   For a list of supported node specifications, see [Alibaba Cloud Elasticsearch pricing information](https://www.aliyun.com/price/product?spm=a2c4g.11186623.2.10.653c6c88NcQPZY#/elasticsearch/detail).

Example:

```

     { "paymentType": "postpaid", "nodeAmount": "3", "instanceCategory": "x-pack", "esAdminPassword": "es_password", "esVersion": "6.7_with_X-Pack", "nodeSpec": { "spec": "elasticsearch.sn2ne.xlarge", "disk": "20", "diskType": "cloud_ssd" }, "networkConfig": { "type": "vpc", "vpcId": "vpc-bp16k1dvzxtmagcva****", "vsArea": "cn-hangzhou-i", "vswitchId": "vsw-bp1k4ec6s7sjdbudw****" } } 
   
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|838D9D11-8EEF-46D8-BF0D-BC8FC2B0C2F3|The ID of the request. |
|Result|Struct| |The returned results. |
|instanceId|String|es-cn-t57p81n7ai89v\*\*\*\*|The ID of the ECS instance. |

## Examples

Sample requests

```

     POST /openapi/instances HTTP/1.1 public request headers 
   
```

Sample success responses

`JSON` format

```

     { "Result": { "instanceId": "es-cn-t57p81n7ai89v****" }, "RequestId": "838D9D11-8EEF-46D8-BF0D-BC8FC2B0****" } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

