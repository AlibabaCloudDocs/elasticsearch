# DiagnoseInstance

Call the DiagnoseInstance to diagnose the instance immediately.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DiagnoseInstance&type=ROA&version=2017-06-13)

## Request headers

This operation uses only common request parameters, and does not involve special request headers. For more information, see the topic about common parameters.

## Request syntax

```
POST /openapi/diagnosis/instances/[InstanceId]/actions/diagnose HTTP/1.1
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance. |
|lang|String|Query|No|en|Language configuration. Multiple languages are supported. |
|ClientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |

## RequestBody

Enter the following parameters in RequestBody to specify the diagnosis task information.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|type

|String

|Yes

|ALL

|The type of the diagnostic task. Valid values:

ALL: ALL indexes are diagnosed.

SELECT: diagnoses the selected indexes. |
|indices

|List<String\\\>

|Yes

|\["library"\]

|The list of diagnostic indexes. **type** is set to **ALL**, indices can be left empty \(\[\]\). |
|diagnoseItems

|List<String\\\>

|Yes

|\["ClusterBulkRejectDiagnostic",...\]

|The diagnostic item. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|createTime|Long|1535745731000|The timestamp when the diagnostic report was generated. |
|diagnoseItems|Array of diagnoseItems| |The list of report diagnostic item information. |
|item|String|\[\]|The asynchronous diagnosis task. A null value is returned. The return value of this field is currently not open and can be ignored. |
|instanceId|String|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the instance to be diagnosed. |
|reportId|String|trigger\_\_2020-08-17T17:09:02|The ID of the report. |
|state|String|RUNNING|The diagnostic status. Supported: SUCCESS, FAILED, and RUNNING. |

## Examples

Sample requests

```
POST /openapi/diagnosis/instances/es-cn-n6w1o1x0w001c ***** /actions/diagnose HTTP/1.1 
common request header
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

Sample success responses

`XML` format

```
<Result>
    <reportId>trigger__2020-10-19T16:49:56</reportId>
    <instanceId>es-cn-n6w1o1x0w001c****</instanceId>
    <state>RUNNING</state>
    <createTime>0</createTime>
</Result>
<RequestId>1EE5BB1E-7ECE-4CFE-A05A-7F1EE2C4****</RequestId>
```

`JSON` format

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

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

