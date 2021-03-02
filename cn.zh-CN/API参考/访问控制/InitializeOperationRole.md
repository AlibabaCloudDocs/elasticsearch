# InitializeOperationRole

调用InitializeOperationRole，创建服务关联角色。

**说明：** 通过采集器采集不同数据源的日志，或执行集群弹性扩缩容任务（是适用于中国站）时，需要先授权创建服务关联角色。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=InitializeOperationRole&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/user/slr HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定待创建的服务关联角色的名称。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|rolename

|String

|是

|AliyunServiceRoleForElasticsearchCollector

|服务关联角色名称。可选值： AliyunServiceRoleForElasticsearchOps（执行集群弹性扩缩容任务角色，只适用于中国站）、AliyunServiceRoleForElasticsearchCollector（创建和管理Beats采集器）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|29101430-4797-4D1D-96C3-9FCBCCA8F845|请求ID。 |
|Result|Boolean|true|返回结果。支持：

 -   true：创建成功
-   false：创建失败 |

## 示例

请求示例

```
POST /openapi/user/slr HTTP/1.1
公共请求头
{
  "rolename": "AliyunServiceRoleForElasticsearchCollector"
}
```

正常返回示例

`XML`格式

```
<Result>true</Result>
<RequestId>29101430-4797-4D1D-96C3-9FCBCCA8F845</RequestId>
```

`JSON`格式

```
{
	"Result": true,
	"RequestId": "29101430-4797-4D1D-96C3-9FCBCCA8F845"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

