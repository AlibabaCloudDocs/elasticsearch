# ListShardRecoveries

调用ListShardRecoveries，返回有关正在进行和已完成的分片恢复的数据进度列表，默认返回正在进行的分片恢复信息。

**说明：** 分片恢复是从主分片同步到副分片的过程。恢复完成后，副分片可供搜索。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListShardRecoveries&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/cat-recovery HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-7mz293m9a003j\*\*\*\*|实例ID。 |
|activeOnly|Boolean|Query|否|true|显示分片数据恢复跟踪情况，取值含义如下：

 -   true：显示正在进行中的分片数据恢复跟踪情况。
-   false：显示全部的分片数据恢复跟踪情况。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC47D9|请求ID。 |
|Result|Array of Result| |返回结果。 |
|bytesPercent|String|80%|数据恢复进度。 |
|bytesTotal|Long|12086|数据恢复总量。 |
|filesPercent|String|80.0%|文件执行进度。 |
|filesTotal|Long|79|文件总数。 |
|index|String|my-index-000001|索引名称。 |
|sourceHost|String|192.168.XX.XX|源节点IP。 |
|sourceNode|String|2Kni3dJ|源节点。 |
|stage|String|done|数据恢复阶段状态。取值含义如下：

 -   done：执行完毕。
-   finalize：清理工作。
-   index：读取索引元数据并将字节从源复制到目标。
-   init：恢复尚未开始。
-   start：开始恢复。
-   translog：重做事务日志。 |
|targetHost|String|192.168.XX.XX|目标节点IP。 |
|targetNode|String|YVVKLmW|目标节点。 |
|translogOps|Long|12086|待恢复的Translog操作的数量。 |
|translogOpsPercent|String|80%|恢复Translog操作的进度。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-7mz293m9a003j****/cat-recovery HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC47D9",
    "Result": [
        {
            "index": "my-index-000001",
            "sourceHost": "192.168.XX.XX",
            "sourceNode": "2Kni3dJ",
            "targetHost": "192.168.XX.XX",
            "targetNode": "YVVKLmW",
            "stage": "index",
            "filesTotal": 79,
            "filesPercent": "80.0%",
            "bytesTotal": 12086,
            "bytesPercent": "80%",
            "translogOps": 12086,
            "translogOpsPercent": "80%"
        },
        {
            "index": "my-index-000002",
            "sourceHost": "192.168.XX.XX",
            "sourceNode": "2Kni3dJ",
            "targetHost": "192.168.XX.XX",
            "targetNode": "YVVKLmW",
            "stage": "index",
            "filesTotal": 56,
            "filesPercent": "100%",
            "bytesTotal": 11086,
            "bytesPercent": "100%",
            "translogOps": 11086,
            "translogOpsPercent": "100%"
        }
    ]
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

