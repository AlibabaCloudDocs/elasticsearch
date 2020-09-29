# ResumeLogstashTask

调用ResumeLogstashTask，恢复实例的变更中断任务。恢复后实例会进入生效中（activating）状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ResumeLogstashTask&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/logstashes/[InstanceId]/actions/resume HTTPS|HTTP
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|InstanceId|String|是|ls-cn-4591f1y6\*\*\*\*|实例ID。 |
|clientToken|String|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|InstanceNotFound|错误码 ，正常调用时，不会返回该参数。 |
|Message|String|The specified cluster does not exist. Check the cluster status and try again.|错误信息，正常调用时，不会返回该参数。 |
|RequestId|String|0FA05123-745C-42FD-A69B-AFF48EF9\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：恢复任务成功
-   false：恢复任务失败 |

## 示例

请求示例

```
POST /openapi/logstashes/ls-cn-4591f1y6****/actions/resume HTTP/1.1
公共请求头
```

正常返回示例

`XML` 格式

```
<Result>true</Result>
<RequestId>0FA05123-745C-42FD-A69B-AFF48EF9****</RequestId>
```

`JSON` 格式

```
{
    "Result": true,
    "RequestId": "0FA05123-745C-42FD-A69B-AFF48EF9****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

