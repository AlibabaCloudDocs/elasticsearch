# ListDataTasks

Call ListDataTasks to obtain the information of a data migration task.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListDataTasks&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/instances/[InstanceId]/data-task HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|InstanceId|String|Yes|es-cn-oew1oxiro000f\*\*\*\*|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|createTime|String|2020-07-30 06:32:18|The time when the site monitoring task was created. |
|sinkCluster|Struct| |The information of the target cluster. |
|dataSourceType|String|1|The type of the target cluster. Default value: elasticsearch. |
|endpoint|String|http://192.168.xx.xx:4101|The public network access address of the target cluster. |
|index|String|product\_info|The target index. |
|type|String|\_doc|The type of the destination index. |
|vpcId|String|vpc-2ze55voww95g82gak\*\*\*\*|The ID of the VPC to which the cluster belongs. |
|vpcInstanceId|String|es-cn-09k1rnu3g0002\*\*\*\*-worker|The instance ID or Server Load Balancer \(SLB\) ID of the current cluster. |
|vpcInstancePort|String|9200|The access port number of the cluster. |
|sourceCluster|Struct| |The information about the source cluster. |
|dataSourceType|String|1|The type of the source cluster. Default value: elasticsearch. |
|index|String|product\_info|The index whose data you want to migrate. |
|mapping|String|\{\\"\_doc\\":\{\\"properties\\":\{\\"user\\":\{\\"properties\\":\{\\"last\\":\{\\"type\\":\\"text\\",...\}\}\}\}\}\}|The Mapping configuration of the cluster. |
|routing|String|\_id|The routing field to index the table. It is set to the primary key by default. |
|settings|String|\{\\n \\"index\\": \{\\n \\"replication\\": \{\\n\}.....\}\}|The Settings of the cluster. |
|type|String|\_doc|The type of the destination index. |
|status|String|SUCCESS|The status of the task. |
|taskId|String|et\_cn\_mfv1233r47272\*\*\*\*|The ID of the task. |

## Examples

Sample requests

```
GET /openapi/instances/es-cn-oew1oxiro000f****/data-task HTTP/1.1
Common request header
```

Sample success responses

`JSON` format

```
{
    "Result": [
        {
            "taskId": "et_cn_mfv1233r47272****",
            "sourceCluster": {
                "dataSourceType": "elasticsearch",
                "endpoint": "http://192.168.xx.xx:4101",
                "vpcInstancePort": 9200,
                "vpcId": "vpc-2ze55voww95g82gak****",
                "vpcInstanceId": "es-cn-09k1rnu3g0002****-worker",
                "index": "product_info",
                "type": "products"
            },
            "sinkCluster": {
                "dataSourceType": "elasticsearch",
                "index": "product_info01",
                "type": "_doc",
                "settings": "{\n  \"index\": {\n    \"replication\": {\n}.....}}",
                "mapping": "{\"_doc\":{\"properties\":{\"user\":{\"properties\":{\"last\":{\"type\":\"text\",...}}}}}}"
                },
            "status": "SUCCESS",
            "createTime": "2020-08-03 08:36:19"
        },
        {
            "taskId": "et_cn_vb9g57i4h4eyp****",
            "sourceCluster": {
                "dataSourceType": "elasticsearch",
                "endpoint": "http://192.168.xx.xx:4096",
                "vpcInstancePort": 9200,
                "vpcId": "vpc-2ze55voww95g82gak****",
                "vpcInstanceId": "es-cn-oew1oxiro000f****-worker",
                "index": "my_index",
                "type": "_doc"
            },
            "sinkCluster": {
                "dataSourceType": "elasticsearch",
                "index": "my_index003",
                "type": "_doc",
                "settings": "{\n  \"index\": {\n    \"replication\": {\n}.....}}",
                "mapping": "{\"_doc\":{\"properties\":{\"user\":{\"properties\":{\"last\":{\"type\":\"text\",...}}}}}}"
                },
            "status": "SUCCESS",    
            "createTime": "2020-07-30 06:32:18"
        }
    ],
    "RequestId": "8FB71A9A-1ACE-40DA-ADC0-2B3DB44F****"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

