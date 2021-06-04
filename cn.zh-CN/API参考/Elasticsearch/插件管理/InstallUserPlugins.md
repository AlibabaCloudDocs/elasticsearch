# InstallUserPlugins

调用InstallUserPlugins，安装用户自定义的已经上传至Elasticsearch控制台的插件。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=InstallUserPlugins&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/plugins/user/actions/install HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-i7m27ausp001l\*\*\*\*|实例ID。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定自定义插件列表。

|参数

|类型

|是否必须

|示例值

|描述 |
|----|----|------|-----|----|
|RequestBody

|Array

| | | |
|└ name

|String

|是

|pluginName1.zip

|用户自定义的已经上传至Elasticsearch控制台的插件。 |

└表示子参数。

示例如下：

```

[
    {"name": "pluginName1.zip"},
    {"name": "pluginName2.zip"}
]


```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6F\*\*\*\*\*|请求ID。 |
|Result|List|\["pluginName1.zip", "pluginName2.zip"\]|请求安装的插件列表。如果安装失败，系统会返回对应的错误码，请参考错误码进行问题定位。 |

## 示例

请求示例

```
POST /openapi/instances/[es-cn-i7m27ausp001l****]/plugins/user/actions/install HTTP/1.1
公共请求头
```
[
    {"name": "pluginName1.zip"},
    {"name": "pluginName2.zip"}
]
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6F*****",
    "Result": "[\"pluginName1.zip\", \"pluginName2.zip\"]"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

