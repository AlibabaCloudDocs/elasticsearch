# ListDefaultCollectorConfigurations

调用ListDefaultCollectorConfigurations，获取采集器的默认配置文件。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDefaultCollectorConfigurations&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/beats/default-configurations HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|resType|String|Query|是|fileBeat|采集器类型。可选值：

 -   fileBeat
-   metricBeat
-   heartBeat
-   auditBeat |
|resVersion|String|Query|是|6.8.5\_with\_community|采集器版本。采集器部署的机器类型不同，可选的版本也不同，具体说明如下：

 -   ECS：6.8.5\_with\_community
-   ACK：6.8.13\_with\_community |
|sourceType|String|Query|否|ECS|指定采集器部署机器的类型，不填则返回全部。可选值：

 -   ECS：ECS服务器
-   ACK：容器Kubernetes集群 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|8BAE3C32-8E4A-47D6-B4B0-95B5DE643BF5|请求ID。 |
|Result|Array of Result| |返回结果。 |
|content|String|- key: log\\n title: Log file content\\n description: \>\\n Contains log file lines.\\n fields:\\n ......|配置文件内容。 |
|fileName|String|fields.yml|配置文件名称。 |

## 示例

请求示例

```
GET /openapi/beats/default-configurations?resVersion=6.8.5_with_community&resType=fileBeat&sourceType=ECS HTTP/1.1
公共请求头
```

正常返回示例

`XML`格式

```
<Result>
    <fileName>fields.yml</fileName>
    <content>- key: log
  title: Log file content
  description: &gt;
    Contains log file lines.
  fields:
 ......</content>
</Result>
<Result>
    <fileName>filebeat.yml</fileName>
    <content>###################### Filebeat Configuration Example #########################

# This file is an example configuration file ......</content>
</Result>
<RequestId>8BAE3C32-8E4A-47D6-B4B0-95B5DE643BF5</RequestId>
```

`JSON`格式

```
{
	"Result": [
		{
			"fileName": "fields.yml",
			"content": "- key: log\n  title: Log file content\n  description: >\n    Contains log file lines.\n  fields:\n ......"
		},
		{
			"fileName": "filebeat.yml",
			"content": "###################### Filebeat Configuration Example #########################\n\n# This file is an example configuration file ......"
		}
	],
	"RequestId": "8BAE3C32-8E4A-47D6-B4B0-95B5DE643BF5"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

