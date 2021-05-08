# ListDiagnoseReport

Call ListDiagnoseReport to obtain the historical reports of intelligent maintenance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListDiagnoseReport&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request structure

```
GET /openapi/diagnosis/instances/[InstanceId]/reports HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|endTime|Long|Successful|1595174399999|The end time of the query. |
|InstanceId|String|Yes|es-cn-n6w1qu7ei000p\*\*\*\*|The ID of an instance. |
|startTime|Long|Successful|1594569600000|The start time of the query. |
|lang|String|No|spanish|The language of the report obtained. |
|page|Integer|No|1|The page number of the returned page. |
|size|Integer|No|20|Number of reports per page. |
|detail|Boolean|No|true|Indicates whether to display the diagnostic item details. |
|trigger|String|No|SYSTEM|The trigger method of the health diagnosis. Valid values: SYSTEM \(automatically triggered by the SYSTEM\), INNER \(internally triggered\), and USER \(manually triggered by the USER\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. |
|X-Total-Count|Integer|15|The total number of entries returned. |
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|createTime|Long|1535745731000|The timestamp when the report was created. |
|diagnoseItems|Array of diagnoseItems| |The list of diagnostic item information. |
|detail|Struct| |The diagnostic item details. |
|desc|String|Check whether the number of replica shards is optimal and easy to maintain|The description of the diagnostic item. |
|name|String|Number of Replica Shards|The full name of the diagnostic item. |
|result|String|You may need to adjust the numbers of replica shards of some indices as follows: \[geoname08 : 0 -\> 1\]\[geoname09 : 0 -\> 1\]\[geonametest01 : 0 -\> 1\]|The diagnosis result. |
|suggest|String|You can call the following function in the Elasticsearch API....|Diagnostic Recommendations. |
|type|String|ES\_API|The type of the diagnostic result. Supported: TEXT, CONSOLE\_API, and ES\_API. |
|health|String|YELLOW|The health status of the diagnostic item. Valid values: GREEN, YELLOW, RED, and UNKNOWN. |
|item|String|IndexAliasUseDiagnostic|The name of the diagnostic item. |
|health|String|YELLOW|The overall health of the cluster in the report. Valid values: GREEN, YELLOW, RED, and UNKNOWN. |
|instanceId|String|es-cn-abc|The ID of the diagnosed instance. |
|reportId|String|trigger\_\_2020-08-17T17:09:02f|Report ID. |
|state|String|SUCCESS|The diagnostic status. Valid values: SUCCESS, FAILED, and RUNNING. |
|trigger|String|USER|The trigger method of health diagnosis. Valid values: SYSTEM \(automatically triggered by the SYSTEM\), INNER \(internally triggered\), and USER \(manually triggered by the USER\). |

## Examples

Sample requests

```
diagnosis/instances/es-cn-09k1rocex0006****/reports? startTime=1600099200000&endTime=1600185600000
Common request header
```

Sample success responses

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

