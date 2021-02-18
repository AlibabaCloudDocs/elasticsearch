# ModifyDeployMachine

调用ModifyDeployMachine，更新采集器安装的ECS机器。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ModifyDeployMachine&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/collectors/[ResId]/actions/modify-deploy-machines HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ResId|String|Path|是|ct-cn-xb1i7q79u65nk\*\*\*\*|采集器ID。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定目标ECS实例信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|machines

|List

|是

| |目标ECS实例信息。 |
|└instanceId

|String

|是

|i-bp11u91xgubypcuz\*\*\*\*

|实例ID。 |
|type

|String

|是

|ECSInstanceId

|采集器部署的机器类型。仅支持ECSInstanceId（ECS机器部署）。 |
|configType

|String

|是

|collectorDeployMachine

|配置类型。仅支持collectorDeployMachine（采集器的部署机器）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|C37CE536-6C0F-4778-9B59-6D94C7F7EB63|请求ID。 |
|Result|Boolean|true|是否更新成功：

 -   true：成功
-   false：失败 |

## 示例

请求示例

```
POST /openapi/collectors/ct-cn-xb1i7q79u65nk****/actions/modify-deploy-machines HTTP/1.1
公共请求头
{
  "machines":[
      {"instanceId":"i-bp1ei8ysh7orb6eq****"},
      {"instanceId":"i-bp12plyjhrv7eobp****"}
  ],
  "type":"ECSInstanceId",
  "configType":"collectorDeployMachine"
}
```

正常返回示例

`XML`格式

```
<Result>true</Result>
<RequestId>C37CE536-6C0F-4778-9B59-6D94C7F7EB63</RequestId>
```

`JSON`格式

```
{
  "Result": true,
  "RequestId": "C37CE536-6C0F-4778-9B59-6D94C7F7EB63"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

