# CreateLogstash

调用CreateLogstash，创建一个Logstash实例。

在调用接口前，请注意：

-   请确保在调用该接口前，已充分了解Logstash产品的付费方式和价格。

    详细信息，请参见[产品定价](~~139428~~)。

-   创建实例需要通过实名认证。

    详细信息，请参见[实名认证](~~37175~~)。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=CreateLogstash&type=ROA&version=2017-06-13)

## 请求头

POST /openapi/logstashes

## 请求语法

```
POST /openapi/logstashes HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定待创建的Logstash实例的信息。

|参数

|类型

|示例值

|描述 |
|----|----|-----|----|
|paymentType

|String

|prepaid

|实例的付费模式。可选值：prepaid（包年包月）、postpaid（按量付费）。 |
|version

|String

|6.7.0\_with\_X-Pack

|实例版本。目前仅支持6.7\_with\_X-Pack、7.4\_with\_X-Pack。 |
|nodeAmount

|Integer

|2

|实例所包含的节点个数。 |
|nodeSpec

|Struct

| |数据节点的配置信息。 |
|└disk

|Integer

|50

|节点磁盘大小。 |
|└diskType

|String

|cloud\_ssd

|磁盘类型。可选值：cloud\_ssd、cloud\_efficiency。 |
|└spec

|String

|elasticsearch.sn2ne.large

|实例规格。 |
|networkConfig

|Struct

| |网络配置。 |
|└type

|String

|vpc

|网络类型，目前仅支持专有网络。 |
|└vpcId

|String

|vpc-bp16k1dvzxtmagcva\*\*\*\*

|专有网络ID。 |
|└vswitchId

|String

|vsw-bp1k4ec6s7sjdbudw\*\*\*\*

|交换机ID。 |
|└vsArea

|String

|cn-hangzhou-i

|实例所在的可用区。 |

**说明：** └表示子参数。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE\*\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|instanceId|String|ls-cn-n6w1o5jq\*\*\*\*|实例ID。 |

## 示例

请求示例

```
https://elasticsearch.cn-hangzhou.aliyuncs.com/openapi/logstashes
公共请求头
{
  "paymentType": "postpaid",
  "version": "6.7_with_X-Pack",
  "nodeAmount": "2",
  "networkConfig": {
    "type": "vpc",
    "vpcId": "vpc-bp16k1dvzxtmagcva****",
    "vsArea": "cn-hangzhou-i",
    "vswitchId": "vsw-bp1k4ec6s7sjdbudw****"
  },
  "nodeSpec": {
    "disk": "20",
    "spec": "elasticsearch.sn1ne.large",
    "diskType": "cloud_ssd"
  }
}
```

正常返回示例

`JSON`格式

```
{
  "Result": {
    "instanceId": "ls-cn-7g1umu96oit2e****"
  },
  "RequestId": "2E6C4DA1-F8E4-4C64-8F07-6D03D6D7553D"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

