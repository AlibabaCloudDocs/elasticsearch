# ListDataStreams

调用ListDataStreams，用于查看数据流列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDataStreams&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/data-streams HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif24adwc0082\*\*\*\*|实例ID。 |
|name|String|Query|否|Log1|数据流名称。 |
|isManaged|Boolean|Query|否|false|是否只显示托管中的索引，取值含义如下：

 -   true：只显示托管中的索引。
-   false（默认值）：显示全部索引。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Managed-Count|Integer|100|数据流总个数。 |
|X-Managed-StorageSize|Long|143993923932990|索引存储总大小，单位：Byte。 |
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回的数据流详情。 |
|health|String|Green|数据流状态，支持以下三种状态：

 -   Green：健康。
-   Yellow：报警。
-   Red：异常。 |
|ilmPolicyName|String|rollver1|生命周期策略名称。 |
|indexTemplateName|String|template1|索引模版名称。 |
|indices|Array of indices| |当前数据流下的索引信息。 |
|createTime|String|2018-07-13T03:58:07.253Z|查询数据流列表的时间。 |
|health|String|Green|索引状态，支持以下三种状态：

 -   Green：健康。
-   Yellow：报警。
-   Red：异常。 |
|isManaged|Boolean|false|该字段已废弃，无需关注。 |
|managedStatus|String|following|索引托管状态，支持以下三种状态：

 -   following：托管中。
-   closing：取消托管中。
-   closed：未托管。 |
|name|String|Log1|数据流名称。 |
|size|Long|15393899|当前索引所占用的总存储空间。单位：Byte。 |
|managedStorageSize|Long|1788239393298|当前数据流下的托管索引，所占用的总存储空间。单位：Byte。 |
|name|String|my-index-0001|索引名称。 |
|totalStorageSize|Long|1788239393298|当前数据流下的全部索引，所占用的总存储空间。单位：Byte。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-nif24adwc0082****/data-streams HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "F99407AB-2FA9-489E-A259-40CF6DCC****", 
    "Result": [
        {
            "name": "Log1", 
            "health": "GREEN", 
            "indices": [
                {
                    "name": "my-index-0001", 
                    "health": "green", 
                    "size": 15393899, 
                    "createTime": "2018-07-13T03:58:07.253Z", 
                    "managedStatus": "following"
                }, 
                {
                    "name": "my-index-0001", 
                    "health": "green", 
                    "size": 14958382, 
                    "createTime": "2018-07-13T03:58:07.253Z", 
                    "managedStatus": "closed"
                }, 
                {
                    "name": "my-index-0001", 
                    "health": "green", 
                    "size": 34939228, 
                    "createTime": "2018-07-13T03:58:07.253Z", 
                    "managedStatus": "closing"
                }
            ], 
            "totalStorageSize": 1788239393298, 
            "managedStorageSize": 1788239393298, 
            "indexTemplateName": "template1", 
            "ilmPolicyName": "rollver1"
        }
    ], 
    "Headers": {
        "X-Managed-Count": 100, 
        "X-Managed-StorageSize": 143993923932990
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

