# DiagnoseInstance

调用DiagnoseInstance，即刻诊断实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DiagnoseInstance&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/diagnosis/instances/[InstanceId]/actions/diagnose HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|lang|String|Query|否|en|语言配置，支持多种语言。 |
|ClientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定诊断任务信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|type

|String

|是

|ALL

|诊断任务类型，支持：

 ALL：诊断所有索引。

 SELECT：诊断选定的索引。 |
|indices

|List<String\\\>

|是

|\["library"\]

|诊断索引列表。**type**为**ALL**时，indices可设置为空（\[\]）。 |
|diagnoseItems

|List<String\\\>

|是

|\["ClusterBulkRejectDiagnostic",...\]

|诊断项。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|createTime|Long|1535745731000|诊断报告生成的时间戳。 |
|diagnoseItems|Array of diagnoseItems| |报告诊断项信息列表。 |
|item|String|\[\]|异步诊断任务，返回空值。暂不开放该字段返回值，可忽略。 |
|instanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|诊断的实例ID。 |
|reportId|String|trigger\_\_2020-08-17T17:09:02|报告ID。 |
|state|String|RUNNING|诊断状态。支持：SUCCESS、FAILED和RUNNING。 |

## 示例

请求示例

```
POST /openapi/diagnosis/instances/es-cn-n6w1o1x0w001c****/actions/diagnose HTTP/1.1
公共请求头
 {
    "indices":[],
    "type":"ALL",
    "diagnoseItems":[
        "ClusterBulkRejectDiagnostic",
        "IndexAliasUseDiagnostic",
        "ClusterColorStatusDiagnostic",
        "ClusterDiskResourceDiagnostic",
        "IndexRecoverySlowDiagnostic",
        "IndexReplicaDiagnostic",
        "KibanaIndexToMuchDiagnostic",
        "MasterNodeHighLoadDiagnostic",
        "NodeLeftDiagnostic",
        "NodeLoadDeviationDiagnostic",
        "ClusterComputingResourceDiagnostic",
        "NodeShardsToMuchDiagnostic",
        "IndexRegexpDeleteDiagnostic",
        "IndexSegmentsDiagnostic",
        "IndexShardsDiagnostic",
        "ClusterMinMasterDiagnostic",
        "ClusterStateVersionDiagnostic",
        "ErrorLogDiagnostic",
        "FullGcLogDiagnostic",
        "AutoSnapshotOpenDiagnostic"
    ]
}
```

正常返回示例

`JSON`格式

```
{
	"Result": {
		"reportId": "trigger__2020-10-19T16:49:56",
		"instanceId": "es-cn-n6w1o1x0w001c****",
		"state": "RUNNING",
		"createTime": 0,
		"diagnoseItems": []
	},
	"RequestId": "1EE5BB1E-7ECE-4CFE-A05A-7F1EE2C4****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

