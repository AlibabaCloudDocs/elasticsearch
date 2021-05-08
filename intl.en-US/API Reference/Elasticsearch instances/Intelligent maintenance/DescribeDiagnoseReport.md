# DescribeDiagnoseReport

Call DescribeDiagnoseReport to view the historical reports of intelligent maintenance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeDiagnoseReport&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request structure

```
GET /openapi/diagnosis/instances/[InstanceId]/reports/[ReportId] HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-mp91kzb8m0009\*\*\*\*|The ID of an instance. |
|ReportId|String|Yes|scheduled\_\_2020-09-15T00:40:00|Report ID. Can be passed [ListDiagnoseReportIds](~~183774~~) get through the API. |
|lang|String|No|en|The language in which the intelligent diagnosis report is generated. Multiple languages are supported. Default value: en. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
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
|reportId|String|trigger\_\_2020-08-17T17:09:02|Report ID. |
|state|String|SUCCESS|The diagnostic status. Valid values: SUCCESS, FAILED, and RUNNING. |
|trigger|String|SYSTEM|The trigger method of health diagnosis. Valid values: SYSTEM \(automatically triggered by the SYSTEM\), INNER \(internally triggered\), and USER \(manually triggered by the USER\). |

## Examples

Sample requests

```
GET /openapi/diagnosis/instances/es-cn-09k1rocex0006****/reports/scheduled__2020-09-15T00:40:00? lang=en HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": {
        "reportId": "scheduled__2020-09-15T02:40:00",
        "instanceId": "es-cn-09k1rocex0006****",
        "state": "SUCCESS",
        "trigger": "INNER",
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
            }
        ]
    },
    "RequestId": "7BABD728-1584-432C-A300-25BEBDFC****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

