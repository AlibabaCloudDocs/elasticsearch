# ListPipelineIds

调用ListPipelineIds，设置Kibana管道管理时，测试Logstash与Kibana连通性，并获取目标Kibana上创建的管道ID列表。

**说明：** 管道管理方式分为配置文件管理和Kibana管道管理，部分区域控制台不开放Kibana管道管理。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListPipelineIds&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/pipeline-ids HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|ls-cn-7g1umu96oit2e\*\*\*\*|Logstash实例ID。 |

## RequestBody

RequestBody中还需填入以下参数，用来验证管理管道的Kibana信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|userName

|String

|是

|elastic

|登录Kibana控制台的用户名，默认为elastic。 |
|password

|String

|是

|xxxxxx

|登录Kibana控制台的密码。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果列表。 |
|available|Boolean|true|管道是否可用，取值含义如下：

 -   true：可用。
-   flase：不可用。 |
|code|String|OK|管道不可用错误码。 |
|message|String|OK|管道不可用错误信息。 |
|pipelineId|String|testKibanaManagement|Kibana上创建的管道ID。 |

## 示例

请求示例

```
POST /openapi/instances/ls-cn-7g1umu96oit2e****/pipeline-ids HTTP/1.1
公共请求头
{
    "userName":"elastic",
    "password":"xxxxxx"
}
```

正常返回示例

`JSON`格式

```
{
    "Result":[
        {
            "pipelineId":"testKibanaManagement",
            "available":true,
            "code":"OK",
            "message":"OK"
        }
    ],
    "RequestId":"E50BC6C3-23B5-4CA0-983C-066098FB8E34"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

