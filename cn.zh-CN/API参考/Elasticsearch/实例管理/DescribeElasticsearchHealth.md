# DescribeElasticsearchHealth

调用DescribeElasticsearchHealth，获取指定Elasticsearch实例的健康情况。

实例健康情况，支持以下三种状态：

-   GREEN：主、副分片分配正常。
-   YELLOW：主分片正常分配，副本未正常分配。
-   RED：主分片未正常分配。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeElasticsearchHealth&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/elasticsearch-health HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-tl325wxga000l\*\*\*\*|请求ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|200|响应码。 |
|Message|String|success|响应消息。 |
|RequestId|String|0731F217-2C8A-4D42-8BCD-5C352866E3B7|请求ID。 |
|Result|String|GREEN|返回的实例健康状态。 |

## 示例

请求示例

```
GET /openapi/instances/[es-cn-tl325wxga000l****]/elasticsearch-health HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
  "Result": "GREEN",
  "RequestId": "0731F217-2C8A-4D42-8BCD-5C352866E3B7"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

