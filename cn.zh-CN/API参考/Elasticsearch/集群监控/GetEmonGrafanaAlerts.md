# GetEmonGrafanaAlerts

调用GetEmonGrafanaAlerts，获取Grafana报警列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=GetEmonGrafanaAlerts&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/emon/projects/[ProjectId]/grafana/proxy/api/alerts HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ProjectId|String|Path|是|es-133071096032\*\*\*\*|监控报警项目ID，格式为**es-<yourUID\>**。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|200|响应码。 |
|Message|String|""|响应消息。 |
|RequestId|String|08FA74C7-5654-4309-9729-D555AF587B7F|请求ID。 |
|Success|Boolean|true|请求是否成功：true（成功）、flase（失败）。 |

## 示例

请求示例

```
GET /openapi/emon/projects/es-133071096032****/grafana/proxy/api/alerts HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
  "Success": true,
  "Code": "200",
  "Message": "",
  "Result": {
    "data": [
      {
        "id": 5492,
        "dashboardId": 10981,
        "dashboardUid": "zl-template",
        "dashboardSlug": "esman-ri-zhi-bao-jing",
        "panelId": 10,
        "name": "zl慢查询测试",
        "state": "ok",
        "newStateDate": "2021-05-27T19:02:24+08:00",
        "evalDate": "0001-01-01T00:00:00Z",
        "evalData": {},
        "executionError": "",
        "url": "/d/zl-template/esman-ri-zhi-bao-jing"
      },
      {
        "id": 5127,
        "dashboardId": 10372,
        "dashboardUid": "log-alarm-template",
        "dashboardSlug": "ri-zhi-bao-jing-mo-ban-zuo-ce-hao-importci-da-pan-jsonke-fu-zhi-mo-ren-bao-jing-gui-ze",
        "panelId": 10,
        "name": "慢查询条数报警模板",
        "state": "paused",
        "newStateDate": "2021-05-19T02:59:36+08:00",
        "evalDate": "0001-01-01T00:00:00Z",
        "evalData": {},
        "executionError": "",
        "url": "/d/log-alarm-template/ri-zhi-bao-jing-mo-ban-zuo-ce-hao-importci-da-pan-jsonke-fu-zhi-mo-ren-bao-jing-gui-ze"
      },
      {
        "id": 5491,
        "dashboardId": 10981,
        "dashboardUid": "zl-template",
        "dashboardSlug": "esman-ri-zhi-bao-jing",
        "panelId": 2,
        "name": "慢查询耗时报警",
        "state": "ok",
        "newStateDate": "2021-05-27T12:04:48+08:00",
        "evalDate": "0001-01-01T00:00:00Z",
        "evalData": {},
        "executionError": "",
        "url": "/d/zl-template/esman-ri-zhi-bao-jing"
      },
      {
        "id": 5126,
        "dashboardId": 10372,
        "dashboardUid": "log-alarm-template",
        "dashboardSlug": "ri-zhi-bao-jing-mo-ban-zuo-ce-hao-importci-da-pan-jsonke-fu-zhi-mo-ren-bao-jing-gui-ze",
        "panelId": 2,
        "name": "慢查询耗时超阈值条数报警模板",
        "state": "paused",
        "newStateDate": "2021-05-19T02:59:36+08:00",
        "evalDate": "0001-01-01T00:00:00Z",
        "evalData": {},
        "executionError": "",
        "url": "/d/log-alarm-template/ri-zhi-bao-jing-mo-ban-zuo-ce-hao-importci-da-pan-jsonke-fu-zhi-mo-ren-bao-jing-gui-ze"
      }
    ]
  },
  "RequestId": "DA48E81F-B556-4EE0-BC5D-A280A919CEFB"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

