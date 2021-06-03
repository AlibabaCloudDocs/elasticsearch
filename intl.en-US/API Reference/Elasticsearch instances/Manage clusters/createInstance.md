# createInstance

You can call this operation to create an Elasticsearch instance.

Before calling the interface, note:

-   Make sure that you have read and understand the billing and pricing standards of Alibaba Cloud Elasticsearch.

    For details, see [Alibaba Cloud Elasticsearch pricing](https://www.aliyun.com/price/product?spm=a2c4g.11186623.2.7.657d2cbeRoSPCd#/elasticsearch/detail) .

-   Before you create an instance, you must complete real-name verification.


## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=createInstance&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     POST /openapi/instances HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|clientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

The following parameters must be filled in the RequestBody to specify the information of the instance to be created.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|paymentType

|String

|Yes

|postpaid

|The billing unit of the subscription instance. Optional values: postpaid \(billed by volume\) and prepaid \(package year and month\). |
|paymentInfo

|Map

|No

|postpaid

|The payment details of the subscription instance. |
|└duration

|Integer

|No

|1

|The purchase time. Subscriptions can be purchased on a monthly or yearly basis. |
|└pricingCycle

|String

|No

|Month

|Package year and month unit, support: Year \(year\), Month \(month\). |
|└isAutoRenew

|Boolean

|No

|true

|Whether to turn on the auto-renewal setting, support: true \(on\), false \(not on\). |
|└autoRenewDuration

|Integer

|No

|3

|auto-renewal cycle, unit: month. |
|nodeAmount

|int

|Yes

|3

|The number of data nodes. |
|instanceCategory

|String

|No

|advanced

|Version type. supports advanced \(enhanced\), x-pack \(business\) set advanced, have to buy Master node and CPFS shared storage. |
|esAdminPassword

|String

|Yes

|es\_password

|The access password of the ES instance. requirements include the following characters in three types: uppercase letters, lowercase letters, lowercase letters, numbers, and special characters:!@\#$%^& author \(s\):\_+-=length of from 8 to 32-bit. |
|esVersion

|String

|Yes

|5.5.3\_with\_X-Pack

|The version of the instance. Optional values: 7.10\_with\_X-Pack, 6.7\_with\_X-Pack, 6.7\_with\_A-Pack, 7.7\_with\_X-Pack, 6.8\_with\_X-Pack, 6.3\_with\_X-Pack, 5.6\_with\_X-Pack, 5.5.3\_with\_X-Pack, 7.10\_with\_A-Pack. |
|nodeSpec

|Map

|Yes

| |Data node configurations. |
|└spec

|String

|Yes

|elasticsearch.sn2ne.xlarge

|The node specifications of the cluster. |
|└disk

|String

|Yes

|20

|Single-node storage space, unit: GB. |
|└diskType

|String

|Yes

|cloud\_ssd

|The type of the disk. Optional values: cloud\_ssd\(SSD cloud disk\), cloud\_essd \(Enhanced SSD\), cloud\_efficiency \(efficient cloud disk\). |
|└performanceLevel

|String

|No

|PL1

|The performance level of the ESSD. When the storage type is cloud\_essd, this parameter is required and supports PL1, PL2, and PL3. |
|└diskEncryption

|Boolean

|No

|true

|Whether to enable cloud disk encryption, support: true \(on\), false \(not on\). |
|advancedDedicateMaster

|boolean

|No

|false

|whether to create a dedicated master node. if it is deployed in multiple zones, you must select. |
|masterConfiguration

|Map

|No

| |The exclusive master node configuration. Required when the advancedDedicateMaster is true. |
|└spec

|String

|Yes

|elasticsearch.sn2ne.xlarge

|The node specifications of the cluster. |
|└amount

|int

|Yes

|3

|The number of nodes, currently fixed at 3. |
|└disk

|int

|Yes

|20

|The size of the single-node storage space. Currently, only 20GB is supported. |
|└diskType

|string

|Yse

|cloud\_ssd

|The node storage type. Optional values: cloud\_ssd\(SSD cloud disk\) and cloud\_essd \(Enhanced SSD\). |
|warmNode

|boolean

|No

|false

|Whether to purchase cold data nodes. |
|warmNodeConfiguration

|Map

|No

| |Cold data node configurations. Required when warmNode is true. |
|└spec

|string

|Yse

|elasticsearch.ic5.large

|The node specifications of the cluster. |
|└amount

|Integer

|Yes

|2

|The number of nodes. |
|└diskType

|string

|Yse

|cloud\_efficiency

|The node storage type. Optional value: cloud\_efficiency \(efficient cloud disk\). |
|└disk

|Integer

|Yes

|500

|Single-node storage space. |
|└diskEncryption

|Boolean

|No

|true

|Whether to enable cloud disk encryption, support: true \(on\), false \(not on\). |
|haveClientNode

|boolean

|No

|false

|Whether to purchase the coordination node. |
|clientNodeConfiguration

|Map

|No

| |Coordinate node configurations. Required when haveClientNode is true. |
|└spec

|string

|Yse

|elasticsearch.ic5.large

|The node specifications of the cluster. |
|└amount

|Integer

|Yes

|2

|The number of nodes. |
|└diskType

|string

|Yse

|cloud\_efficiency

|The node storage type. Optional value: cloud\_efficiency \(efficient cloud disk\). |
|└disk

|Integer

|Yes

|20

|The size of the single-node storage space. |
|haveElasticDataNode

|boolean

|No

|false

|Whether to purchase elastic nodes. Before you purchase an elastic node, you need to purchase an exclusive master node. |
|elasticDataNodeConfiguration

|Map

|No

| |Elastic node configurations. Required when the haveElasticDataNode is true. |
|└spec

|string

|Yse

|elasticsearch.ic5.large

|The name of the specification. |
|└amount

|Integer

|Yes

|2

|The number of risk items. |
|└diskType

|string

|Yse

|cloud\_efficiency

|The node storage type. Optional values: cloud\_ssd\(SSD cloud disk\), cloud\_essd \(Enhanced SSD\), cloud\_efficiency \(efficient cloud disk\). |
|└disk

|Integer

|Yes

|20

|The size of the single-node storage space. |
|└performanceLevel

|String

|No

|PL1

|The performance level of the ESSD. When the storage type is cloud\_essd, this parameter is required and supports PL1, PL2, and PL3. |
|└diskEncryption

|Boolean

|No

|true

|Whether to enable cloud disk encryption, support: true \(on\), false \(not on\). |
|haveKibana

|boolean

|No

|true

|Whether to purchase kibana nodes. |
|kibanaConfiguration

|Map

|No

| |Kibana node configurations. Required when haveKibana is true. |
|└spec

|String

|Yes

|elasticsearch.n4.small

|The specification of data nodes. |
|└amount

|Integer

|Yes

|1

|The number of nodes, currently fixed at 1. |
|└disk

|Integer

|Yes

|0

|The storage size, currently fixed at 0. |
|networkConfig

|Map

|Yes

| |The network configurations. |
|└type

|string

|Yes

|VPC

|The type of network. Only VPC is supported. |
|└vpcId

|string

|Yse

|vpc-bp16k1dvzxtmagcva\*\*\*\*

|The ID of the VPC. |
|└vsArea

|string

|Yse

|cn-hangzhou-i

|The ID of the zone to which the vSwitch belongs. |
|└vswitchId

|string

|Yse

|vsw-bp1k4ec6s7sjdbudw\*\*\*\*

|The ID of the vSwitch to which the instance is connected. |
|extendConfigs

|list

|No

| |The instance scaling configuration. |
|└configType

|string

|Yse

|sharedDisk

|The configuration type, fixed to sharedDisk \(shared storage\), is applicable only to enhanced instances. |
|└disk

|Integer

|Yes

|5120

|shared storage the disk size. |
|dryRun

|boolean

|No

|true

|Whether to verify the configuration when creating an instance. Optional values: true \(check only, not created\) and false \(check and create\). |

**Note:**

-   └ indicates a child parameter.
-   For a list of supported node specifications, see [Alibaba Cloud Elasticsearch pricing information](https://www.aliyun.com/price/product?spm=a2c4g.11186623.2.10.653c6c88NcQPZY#/elasticsearch/detail) .

Sample template:

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

     POST /openapi/instances HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": { "instanceId": "es-cn-t57p81n7ai89v****" }, "RequestId": "838D9D11-8EEF-46D8-BF0D-BC8FC2B0****" } 
   
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

