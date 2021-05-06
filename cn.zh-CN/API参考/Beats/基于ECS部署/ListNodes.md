# ListNodes

调用ListNodes，查看安装采集器的ECS机器的状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListNodes&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/collectors/[ResId]/nodes HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ResId|String|Path|是|ct-cn-l871nd0u73c45\*\*\*\*|采集器ID。 |
|page|Integer|Query|否|1|返回结果的分页数。 |
|size|Integer|Query|否|10|每页的结果数。 |
|ecsInstanceIds|String|Query|否|i-bp1ei8ysh7orb6eq\*\*\*\*|ECS实例ID列表。 |
|ecsInstanceName|String|Query|否|test|ECS实例名称。 |
|tags|String|Query|否|\[\{"tagKey":"abc","tagValue":"xyz"\}\]|ECS实例的标签信息。必须包含标签键（tagKey）和标签值（tagValue）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|10|返回记录数。 |
|RequestId|String|E1FD7642-7C40-4FF2-9C0F-21F1A1746F70|请求ID。 |
|Result|Array of Result| |返回结果。 |
|agentStatus|String|heartOk|ECS上各采集器的状态。支持：

 -   heartOk：心跳正常
-   heartLost：心跳异常
-   uninstalled：未安装
-   failed：安装失败 |
|cloudAssistantStatus|String|true|是否已开通云助手。支持：

 -   true：开通
-   false：未开通 |
|ecsInstanceId|String|i-bp13y63575oypr\*\*\*\*|ECS实例ID。 |
|ecsInstanceName|String|ECS\_beat|ECS实例名称。 |
|ipAddress|Array of ipAddress| |ECS实例的IP信息列表。 |
|host|String|192.168.xx.xx|IP地址。 |
|ipType|String|public|IP地址类型。支持：

 -   public：公网IP地址
-   private：私网IP地址 |
|osType|String|linux|ECS实例的操作系统类型。支持：

 -   windows：Windows Server
-   linux：Linux |
|status|String|running|ECS实例状态。支持：

 -   running：运行中
-   starting：启动中
-   stopping：停止中
-   stopped：已停止 |
|tags|Array of tags| |ECS实例的标签信息。 |
|tagKey|String|abc|标签键。 |
|tagValue|String|xyz|标签值。 |

## 示例

请求示例

```
GET /openapi/collectors/ct-cn-l871nd0u73c45****/nodes HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
  "Result": [
    {
      "ecsInstanceId": "i-bp13y63575oypr9d****",
      "ecsInstanceName": "zl-test02-keepit",
      "status": "running",
      "ipAddress": [
        {
          "host": "47.111.xx.xx",
          "ipType": "public"
        },
        {
          "host": "10.8.xx.xx",
          "ipType": "private"
        }
      ],
      "tags": [
        {
          "tagKey": "a",
          "tagValue": "b"
        }
      ],
      "agentStatus": "failed",
      "osType": "linux",
      "cloudAssistantStatus": "true"
    }
  ],
  "RequestId": "E1FD7642-7C40-4FF2-9C0F-21F1A1746F70",
  "Headers": {
    "X-Total-Count": 1
  }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

