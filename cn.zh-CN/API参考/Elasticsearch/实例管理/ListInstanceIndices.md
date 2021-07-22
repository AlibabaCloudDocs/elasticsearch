# ListInstanceIndices

调用ListInstanceIndices，获取集群的索引列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListInstanceIndices&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/indices HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-tl329rbpc0001\*\*\*\*|实例ID。 |
|all|Boolean|Query|否|false|是否获取所有索引，取值含义如下：

 -   true：返回包含系统索引在内的索引列表。
-   false（默认值）：返回除系统索引外的索引列表。 |
|name|String|Query|否|.ds-ds--2021.07.21-000002|索引名称。 |
|isManaged|Boolean|Query|否|true|是否只显示托管中的索引，取值含义如下：

 -   true：只显示托管中的索引。
-   false（默认值）：显示全部索引。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Headers|Struct| |返回头信息。 |
|X-Managed-Count|Integer|1|云端托管中的索引总个数。 |
|X-Managed-StorageSize|Long|416|云端托管中的索引总大小。单位：Byte。 |
|RequestId|String|11608354-FBA1-4177-A13A-FAC7E5D0\*\*\*\*|请求ID。 |
|Result|Array of Result| |索引列表详情。 |
|createTime|String|2021-01-11T05:49:41.114Z|查询索引列表的时间。 |
|health|String|green|索引的运行状态，支持以下三种状态：

 -   green：健康。
-   yellow：报警。
-   red：异常。 |
|isManaged|String|true|该参数已废弃，无需关注。 |
|managedStatus|String|following|索引托管状态，支持以下三种状态：

 -   following：托管中。
-   closing：取消托管中。
-   closed：未托管。 |
|name|String|.ds-ds--2021.07.21-000002|索引名称。 |
|size|Long|416|当前索引所占用的总存储空间。单位：Byte。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-tl329rbpc0001****/indices HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
    "Result": [
        {
            "name": ".ds-ds--2021.07.21-000002",
            "health": "green",
            "size": 416,
            "createTime": "2021-07-21T14:40:37.098Z",
            "managedStatus": "following"
        }
    ],
    "RequestId": "11608354-FBA1-4177-A13A-FAC7E5D0F73F",
    "Headers": {
        "X-Managed-Count": 1,
        "X-Managed-StorageSize": 416
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

