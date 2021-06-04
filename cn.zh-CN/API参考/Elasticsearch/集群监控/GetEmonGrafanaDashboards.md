# GetEmonGrafanaDashboards

调用GetEmonGrafanaDashboards，获取Grafana大盘列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=GetEmonGrafanaDashboards&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/emon/projects/[ProjectId]/grafana/proxy/api/search HTTP/1.1
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
|RequestId|String|1E9D9827-2092-4385-9DA1-FC5A8D1DB3F5|请求ID。 |
|Success|Boolean|true|请求是否成功：true（成功）、flase（失败）。 |

## 示例

请求示例

```
GET /openapi/emon/projects/es-133071096032****/grafana/proxy/api/search HTTP/1.1
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
        "id": 26,
        "uid": "elasticsearch-basic",
        "title": "elasticsearch-basic",
        "uri": "db/elasticsearch-basic",
        "url": "/d/elasticsearch-basic/elasticsearch-basic",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      },
      {
        "id": 34,
        "uid": "elasticsearch-log",
        "title": "elasticsearch-log",
        "uri": "db/elasticsearch-log",
        "url": "/d/elasticsearch-log/elasticsearch-log",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      },
      {
        "id": 50,
        "uid": "elasticsearch-server",
        "title": "elasticsearch-server",
        "uri": "db/elasticsearch-server",
        "url": "/d/elasticsearch-server/elasticsearch-server",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      },
      {
        "id": 10981,
        "uid": "zl-template",
        "title": "es慢日志报警",
        "uri": "db/esman-ri-zhi-bao-jing",
        "url": "/d/zl-template/esman-ri-zhi-bao-jing",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      },
      {
        "id": 11062,
        "uid": "lrr-elasticsearch-basic",
        "title": "lrr-elasticsearch-basic",
        "uri": "db/lrr-elasticsearch-basic",
        "url": "/d/lrr-elasticsearch-basic/lrr-elasticsearch-basic",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      },
      {
        "id": 10982,
        "uid": "elasticsearch-zl",
        "title": "zl-basic",
        "uri": "db/zl-basic",
        "url": "/d/elasticsearch-zl/zl-basic",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      },
      {
        "id": 10372,
        "uid": "log-alarm-template",
        "title": "日志报警模板(左侧\"+\"号import此大盘json可复制默认报警规则)",
        "uri": "db/ri-zhi-bao-jing-mo-ban-zuo-ce-hao-importci-da-pan-jsonke-fu-zhi-mo-ren-bao-jing-gui-ze",
        "url": "/d/log-alarm-template/ri-zhi-bao-jing-mo-ban-zuo-ce-hao-importci-da-pan-jsonke-fu-zhi-mo-ren-bao-jing-gui-ze",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      },
      {
        "id": 11015,
        "uid": "User_defined_basic_indicators",
        "title": "自定义基础指标",
        "uri": "db/zi-ding-yi-ji-chu-zhi-biao",
        "url": "/d/User_defined_basic_indicators/zi-ding-yi-ji-chu-zhi-biao",
        "slug": "",
        "type": "dash-db",
        "tags": [],
        "isStarred": false
      }
    ]
  },
  "RequestId": "C0FD346A-56A3-405C-979C-A005A75B9E3B"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

