# GetEmonMonitorData

调用GetEmonMonitorData，查询Elasticsearch实例的Grafana指标监控数据。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=GetEmonMonitorData&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/emon/projects/[ProjectId]/metrics/query HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ProjectId|String|Path|是|es-185320276651\*\*\*\*|项目ID，格式为`es-当前账号ID`。 |

## RequestBody

|参数名

|类型

|是否必填

|示例值

|描述 |
|-----|----|------|-----|----|
|start

|Long

|是

|1522127381471

|指标监控开始的时间戳。单位：毫秒。 |
|expression

|String

|否

|a+b

|指标数据查询表达式。 |
|queries

|List

|是

| |指标查询条件。 |
|└id

|String

|否

|a

|查询条件queries如果用到了表达式expression，queries json数组里会描述expression里参与运算的指标，id就是每个指标在expression里的代数符号，真正的指标名称在queries数组的每个元素里。 |
|└metric

|String

|是

|elasticbuild.elasticsearch.source.total\_doc\_count

|指标名称。 |
|└aggregator

|String

|是

|sum

|指标纵向聚合方式。即对指标进行sum、avg、max、min等聚合运算。 |
|└downsample

|String

|是

|avg

|指标横向方式。即对指标在\[start,end\]窗口内的时间线，在时间维度降精度，例如把20s精度聚合为1min精度。 |
|└tags

|Map

|是

|\[env,dev\]

|指标查询标签。格式为\[key,value\]，key要明确指定，value支持通配符，使用通配符可返回满足条件的多序列数据。 |
|└granularity

|String

|是

|1m

|指标查询的时间精度。范围为\[start,end\]。 |
|limit

|List

|是

|" "

|限定返回结果集的大小。为空则不限制，返回所有查询数据。一般用于tags里value使用通配符，返回结果集非常大的情况。 |
|end

|List

|是

|1522129151000

|指标监控结束的时间戳。单位：毫秒。 |

**说明：** └表示子参数。

示例如下。

```

{
    "start": 1522127381471, 
    "queries": [
    {
        "metric": "elasticbuild.elasticsearch.source.total_doc_count",
        "aggregator": "sum",
        "downsample": "avg", 
        "tags": {
          "taskName": "et-testtsk",
          "userId":"123456" 
        }, 
        "granularity": "1m" 
      }
    ], 
    "limit": "",  
    "end": 1522129151000
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|200|请求状态码。 |
|Message|String|""|请求结果。 |
|RequestId|String|2D184B55-FA51-43F7-A1EF-E68A0545\*\*\*\*|请求ID。 |
|Result|Array of result| |返回结果。 |
|dps|Map|\{ "1586249280": 465.1980465119913, "1586249300": 213.45243650423305 \}|指标实时监控数据。格式为`{时间戳:数据}`。 |
|integrity|Float|1.0|指标查询返回的结果里，时序曲线数据点的完整度。1.0表示100%完整。 |
|messageWatermark|Long|1522127381471|请求到达服务端的时间戳，用于排查问题。 |
|metric|String|elasticbuild.elasticsearch.source.total\_doc\_count|指标名称。 |
|summary|Float|10|queries里如果有通配符，result会包含多条匹配到的时间序列数据，summary是在每个时间点上对这些时间线的value集合，按照query里提供的aggregator类型来聚合。目前聚合方式仅支持avg，后续会逐步支持sum、max、min、median、分位数等。 |
|tags|Map|\{"taskName":"et-xxx","userId":"123456"\}|查询标签。 |
|Success|Boolean|true|请求是否成功：

 -   true：成功
-   false：不成功 |

以下返回示例中，本文只保证包含返回数据列表中的参数，而未提到的参数仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
POST /openapi/emon/projects/es-185320276651****/metrics/query HTTP/1.1
公共请求头
{
    "start": 1522127381471, 
    "queries": [
    {
        "metric": "elasticbuild.elasticsearch.source.total_doc_count",
        "aggregator": "sum",
        "downsample": "avg", 
        "tags": {
          "taskName": "et-testtsk",
          "userId":"123456" 
        }, 
        "granularity": "1m" 
      }
    ], 
    "limit": "",  
    "end": 1522129151000
}
```

正常返回示例

`JSON`格式

```
{
    "Success": true,
    "Code": "200",
    "Message": "",
    "RequestId": "2D184B55-FA51-43F7-A1EF-E68A0545****",
    "Result":[
      {
        "dps":{
          "1583469212":10
        },
        "integrity":1,
        "messageWatermark":1583469212345,
        "metric": "elasticbuild.elasticsearch.source.total_doc_count",
        "summary":10,
        "tags": {
          "taskName": "et-testtsk",
          "userId":"123456" 
        }
      }
    ]
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

