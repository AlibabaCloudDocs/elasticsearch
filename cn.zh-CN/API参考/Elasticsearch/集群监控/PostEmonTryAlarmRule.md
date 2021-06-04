# PostEmonTryAlarmRule

调用PostEmonTryAlarmRule，发送测试的报警消息。

**说明：** 此API接口每小时最多被调用10次。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=PostEmonTryAlarmRule&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/emon/projects/[ProjectId]/alarm-groups/[AlarmGroupId]/alarm-rules/_test HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|AlarmGroupId|String|Path|是|123|从GetEmonGrafanaAlerts接口中获取的报警列表中的ID之一。您可以按需求指定具体的ID。 |
|ProjectId|String|Path|是|es-133071096032\*\*\*\*|监控报警项目ID，格式为**es-<yourUID\>**。 |

**RequestBody**

RequestBody中还需要填入以下参数，用来指定待发送的测试报警消息。

|参数

|类型

|是否必要

|示例值

|描述 |
|----|----|------|-----|----|
|alarmRuleName

|String

|是

|test\_rule

|测试的报警规则名称。 |
|AlarmChannel

|Object

|是

| |通知方式。 |
|└phone

|Boolean

|否

|true

|是否打电话：true（打电话）、flase（不打电话）。 |
|└sms

|Boolean

|否

|true

|是否发短信：true（发短信）、flase（不发短信）。 |
|└dingWebHook

|Boolean

|否

|true

|是否发钉钉消息：true（发钉钉消息）、flase（不发钉钉消息）。 |
|receivers

|List

|是

| |消息接收人列表。 |
|└id

|long

|是

|19

|联系人或者联系人组ID。 |
|└contactGroup

|Boolean

|否

|false

|此ID是否是联系人组ID：true（是联系人组ID）、flase（不是联系人组ID）。 |

**说明：** └表示子参数。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|200|响应码。 |
|Message|String|""|响应消息。 |
|RequestId|String|3EC5731F-0944-4E4F-9DD5-1F976B3FCC3D|请求ID。 |
|Success|Boolean|true|报警消息是否发送成功：true（发送成功）、 false（发送失败）。 |

## 示例

请求示例

```
POST /openapi/emon/projects/es-133071096032****/alarm-groups/123/alarm-rules/_test HTTP/1.1
公共请求头
{
    "alarmRuleName": "test_alarm_rule", 
    "channel": {
        "sms": true, 
        "dingWebHook": true, 
        "phone": true
    }, 
    "receivers": [
        {
            "id": 33, 
            "contactGroup": true
        }, 
        {
            "id": 19, 
            "contactGroup": false
        }
    ]
}
```

正常返回示例

`JSON`格式

```
{
		"Message":"",
		"RequestId":"3EC5731F-0944-4E4F-9DD5-1F976B3FCC3D",
		"Code":"200",
		"Success":true
	}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

