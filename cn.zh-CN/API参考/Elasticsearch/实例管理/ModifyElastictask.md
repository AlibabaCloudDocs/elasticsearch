# ModifyElastictask

调用ModifyElastictask，更新集群弹性扩缩容规则。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ModifyElastictask&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/elastic-task HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-6ja1ro4jt000c\*\*\*\*|实例ID。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定扩缩容信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|elasticExpansionTask

| |否

| |弹性节点扩容规则。 |
|└triggerType

|String

|是

|crontab

|触发条件。可选值：crontab，表示定时触发。 |
|└cronExpression

|String

|是

|0 0 0 ? \* MON

|触发周期，使用Quartz Cron表达式。 |
|└elasticNodeCount

|Integer

|否

|2

|目标高峰期弹性数据节点数量。 |
|└targetIndices

|List

|否

|\["index"\]

|目标弹性索引名称，支持通配符。 |
|└replicaCount

|String

|是

|2

|目标索引的副本数。 |
|elasticShrinkTask

| |否

| |弹性节点缩容规则。 |
|└triggerType

|String

|是

|crontab

|触发条件。可选值：crontab，表示定时触发。 |
|└cronExpression

|String

|是

|4 4 4 ? \* WED

|触发周期，使用Quartz Cron表达式。 |
|└elasticNodeCount

|Integer

|否

|2

|目标低峰期弹性数据节点数量。 |
|└targetIndices

|List

|否

|\["index"\]

|目标弹性索引名称，支持通配符。 |
|└replicaCount

|String

|是

|2

|目标索引的副本数。 |

**说明：** elasticExpansionTask和elasticShrinkTask二者必须选其一，不能都为空。

示例如下。

```

{
    "elasticExpansionTask":
    {
        "triggerType":"crontab",
        "cronExpression":"0 0 0 ? * MON",
        "elasticNodeCount":"2",
        "targetIndices":["*"],
        "replicaCount":"2"
    }
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|elasticExpansionTask|Struct| |弹性节点扩容规则。 |
|cronExpression|String|0 0 0 ? \* MON|触发周期，使用Quartz Cron表达式。 |
|elasticNodeCount|Integer|2|目标高峰期弹性数据节点数量。 |
|replicaCount|Integer|2|目标索引的副本数。 |
|targetIndices|List|\["index"\]|目标弹性索引名称，支持通配符。 |
|triggerType|String|crontab|触发条件。固定为crontab，表示定时触发。 |
|elasticShrinkTask|Struct| |弹性节点缩容规则。 |
|cronExpression|String|4 4 4 ? \* WED|触发周期，使用Quartz Cron表达式。 |
|elasticNodeCount|Integer|2|目标低峰期弹性数据节点数量。 |
|replicaCount|Integer|2|目标索引的副本数。 |
|targetIndices|List|\["index"\]|目标弹性索引名称，支持通配符。 |
|triggerType|String|crontab|触发条件。可选值：crontab，表示定时触发。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-6ja1ro4jt000c****/elastic-task HTTP/1.1
公共请求头
{
    "elasticExpansionTask":
    {
        "triggerType":"crontab",
        "cronExpression":"0 0 0 ? * MON",
        "elasticNodeCount":"2",
        "targetIndices":["*"],
        "replicaCount":"2"
    }
}
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "ECF7F13B-A26F-44E6-B77A-5AD5AC32****",
	"Result": {
		"ElasticExpansionTask": {
			"TriggerType": "crontab",
			"ReplicaCount": 2,
			"CronExpression": "0 0 0 ? * MON",
			"ElasticNodeCount": 2,
			"TargetIndices": [
				"*"
			]
		}
	}
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|404|InstanceNotFound|The specified cluster does not exist. Check the cluster status and try again.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

