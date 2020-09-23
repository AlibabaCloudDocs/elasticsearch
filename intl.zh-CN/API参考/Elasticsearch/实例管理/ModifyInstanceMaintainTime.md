# ModifyInstanceMaintainTime

调用ModifyInstanceMaintainTime，更改并开启实例的可维护时间。

调用此接口前，请注意：

-   在进行正式维护前，阿里云会给您阿里云账号中设置的联系人发送短信和邮件，请注意查收。
-   实例维护当天，为保障整个维护过程的稳定性，实例会在可维护时间段之前进入生效中的状态。当实例处于该状态时，对集群的访问以及查询类操作（如性能监控）不会受到影响，但相关集群变更操作（如集群升配、重启等）均暂时无法使用。
-   在可维护时间段内，实例连接可能发生闪断，请确保应用程序具有重连机制。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ModifyInstanceMaintainTime&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/modify-maintaintime HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|ClientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定可维护时间段信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|maintainStartTime

|String

|否

|02:00Z

|可维护时间段的开始时间，格式：HH:mmZ（UTC时间）。 |
|maintainEndTime

|String

|否

|06:00Z

|可维护时间段的结束时间，格式：HH:mmZ（UTC时间）。 |
|openMaintainTime

|boolean

|是

|true

|是否开启可维护时间段功能。true表示开启，false表示关闭。 |

示例如下。

```

{
    "openMaintainTime": true,
    "maintainStartTime": "03:00Z",
    "maintainEndTime": "04:00Z"
}

```

或

```

{
  "openMaintainTime":false
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/modify-maintaintime HTTP/1.1
公共请求头
{
	"openMaintainTime":true,
	"maintainStartTime":"03:00Z",
	"maintainEndTime":"04:00Z"
}
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>8577468C-D13F-4980-BD71-977F9D82****</RequestId>
```

`JSON` 格式

```
{
	"Result": true,
	"RequestId": "8577468C-D13F-4980-BD71-977F9D82****"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

