# ListDiagnoseReport

调用ListDiagnoseReport，获取智能运维的历史报告。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDiagnoseReport&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/diagnosis/instances/[InstanceId]/reports HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|endTime|Long|Query|是|1595174399999|查询结束时间戳。 |
|InstanceId|String|Path|是|es-cn-n6w1qu7ei000p\*\*\*\*|实例ID。 |
|startTime|Long|Query|是|1594569600000|查询开始时间戳。 |
|lang|String|Query|否|spanish|获取的报告的语言。 |
|page|Integer|Query|否|1|分页数。默认值：1，最小值：1，最大值：200。 |
|size|Integer|Query|否|20|每页报告数。默认值：10，最小值：1，最大值：500。 |
|detail|Boolean|Query|否|true|是否展示诊断项详情。 |
|trigger|String|Query|否|SYSTEM|健康诊断的触发方式，支持：SYSTEM（系统自动触发）、INNER（内部触发）和USER（用户手动触发）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Total-Count|Integer|15|返回的总记录数。 |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|createTime|Long|1535745731000|报告创建的时间戳。 |
|diagnoseItems|Array of diagnoseItems| |报告诊断项信息列表。 |
|detail|Struct| |诊断项详情。 |
|desc|String|Check whether the number of replica shards is optimal and easy to maintain|诊断项说明。 |
|name|String|Number of Replica Shards|诊断项全称。 |
|result|String|You may need to adjust the numbers of replica shards of some indices as follows: \[geoname08 : 0 -&gt; 1\]\[geoname09 : 0 -&gt; 1\]\[geonametest01 : 0 -&gt; 1\]|诊断结果。 |
|suggest|String|You can call the following function in the Elasticsearch API....|诊断建议。 |
|type|String|ES\_API|诊断结果类型。支持：TEXT（文本描述）、CONSOLE\_API（控制台触发型）、ES\_API（API触发型）。 |
|health|String|YELLOW|诊断项的健康度。支持：GREEN、YELLOW、RED和UNKNOWN。 |
|item|String|IndexAliasUseDiagnostic|诊断项名称。 |
|health|String|YELLOW|报告中集群整体的健康度。支持：GREEN、YELLOW、RED和UNKNOWN。 |
|instanceId|String|es-cn-abc|诊断的实例ID。 |
|reportId|String|trigger\_\_2020-08-17T17:09:02f|报告ID。 |
|state|String|SUCCESS|诊断状态。支持：SUCCESS、FAILED和RUNNING。 |
|trigger|String|USER|健康诊断的触发方式。支持：SYSTEM（系统自动触发）、INNER（内部触发）和USER（用户手动触发）。 |

## 示例

请求示例

```
diagnosis/instances/es-cn-09k1rocex0006****/reports?startTime=1600099200000&endTime=1600185600000
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"reportId": "scheduled__2020-09-15T02:40:00",
			"instanceId": "es-cn-09k1rocex0006****",
			"state": "SUCCESS",
			"trigger": "SYSTEM",
			"health": "YELLOW",
			"createTime": 1600108800000,
			"diagnoseItems": [
				{
					"item": "IndexReplicaDiagnostic",
					"health": "YELLOW",
					"detail": {
						"name": "Number of Replica Shards",
						"desc": "Check whether the number of replica shards is optimal and easy to maintain.\nReplica shards can increase the index data reliability and improve the QPS if the resources are sufficient. However, too many replica shards may consume large amounts of disk space and memory. This reduces the performance of write operations.",
						"type": "CONSOLE_API",
						"suggest": "You can call the following function in the Elasticsearch API: \nPUT ${index}/_settings\n{\n    \"settings\": {\n        \"index.number_of_replicas\": \"${num}\"\n    }\n} \r\nSet the index and num parameters to the actual values.",
						"result": "You may need to adjust the numbers of replica shards of some indices as follows: \n[geoname08 : 0 -> 1][geoname09 : 0 -> 1][geonametest01 : 0 -> 1]"
					}
				},
				{
					"item": "IndexShardsDiagnostic",
					"health": "YELLOW",
					"detail": {
						"name": "Number and Sizes of Shards in Each Index",
						"desc": "Check whether the number and sizes of shards in each index are optimal.\nA small number of shards may degrade the read and write performance of an index. A large number of shards consume a lot of system resources and degrade the read and write performance of an index.",
						"type": "ES_API",
						"suggest": "We recommend the following solution: \nhotmovies [size < 1 GB] [7 -> 1, 3]\ngeoname08 [2 GB] [5 -> 1, 3]\ngeoname09 [3 GB] [5 -> 1, 3]\ngeonametest01 [2 GB] [5 -> 1, 3]\n \r\nThis solution applies to the current index sizes. Adjust the number of shards based on the future indices and nodes.",
						"result": "You may need to adjust the number of shards in some indices."
					}
				},
				{
					"item": "NodeLeftDiagnostic",
					"health": "GREEN",
					"detail": {
						"name": "Missing Nodes",
						"desc": "Check whether any node has not joined the cluster.\nMissing nodes may cause serious problems and requires immediate attention.",
						"type": "TEXT",
						"result": "All nodes have joined the cluster."
					}
				},
				{
					"item": "FullGcLogDiagnostic",
					"health": "GREEN",
					"detail": {
						"name": "Full GC Activities",
						"desc": "Check whether the full GC activities of the cluster are normal.",
						"type": "TEXT",
						"result": "The full GC activities of the cluster are normal."
					}
				},
				{
					"item": "ClusterMinMasterDiagnostic",
					"health": "GREEN",
					"detail": {
						"name": "minimum_master_nodes Configuration",
						"desc": "Check whether the discovery.zen.minimum_master_nodes configuration of the cluster is optimal.\nAn improper zen.min.master configuration may cause cluster instability and even cluster split-brain in the case of a high cluster load. Cluster split-brain may cause problems such as missing nodes, cluster jitter, and data exception, which require immediate attention.",
						"type": "TEXT",
						"result": "The  minMaster configuration is optimal. candidateMasterCount is set to 3, and discovery.zen.minimum_master_nodes is set to 2."
					}
				},
				{
					"item": "ClusterStateVersionDiagnostic",
					"health": "GREEN",
					"detail": {
						"name": "Frequent Changes in Cluster Status",
						"desc": "Check whether the cluster status changes frequently.\nFrequent changes in the cluster status can greatly increase the load of the master node, causing frequent GC activities, and may even degrade the cluster performance by blocking the reading and writing of index data.",
						"type": "TEXT",
						"result": "The cluster status changes at an optimal frequency."
					}
				},
				{
					"item": "ErrorLogDiagnostic",
					"health": "GREEN",
					"detail": {
						"name": "Exceptions Log",
						"desc": "Check for exceptions logs.",
						"type": "TEXT",
						"result": "No exceptions log has been detected."
					}
				}
			]
		}
	],
	"RequestId": "40962041-2864-4877-81C7-9657FDA3****",
	"Headers": {
		"X-Total-Count": 1
	}
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

