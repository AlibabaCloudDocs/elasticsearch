# DescribeILMPolicy

调用DescribeILMPolicy，查询指定索引生命周期详情。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=DescribeILMPolicy&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/ilm-policies/[PolicyName] HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-nif24adwc0082\*\*\*\*|实例ID。 |
|PolicyName|String|Path|是|policy-1|索引生命周期策略名称。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|FF44681E-FD41-4FDE-B8DF-295DCDD6\*\*\*\*|请求ID。 |
|Result|Struct| |指定索引生命周期的详情。 |
|name|String|ilm-history-ilm-policy|索引生命周期名称。 |
|phases|Map|\{"cold":\{"minAge":"30d","actions":\{"allocate":\{"numberOfReplicas":1,"require":\{"boxType":"warm"\}\},"setPriority":\{"priority":100\}\}\},"hot":\{"minAge":"0s","actions":\{"rollover":\{"maxAge":"30d","maxDocs":10000,"maxSize":"50gb"\},"setPriority":\{"priority":1000\}\}\},"delete":\{"minAge":"30d","actions":\{"delete":\{\}\}\}\}|索引生命周期内容。 |

**phases字段数据结构说明**

|参数

|类型

|示例值

|是否必选

|描述 |
|----|----|-----|------|----|
|\{key\}

|Struct

| |否

|当前生命周期阶段，支持以下三种阶段：hot（热数据阶段，正在积极更新和查询索引）；cold（冷数据阶段，索引不再被更新并且很少被查询。信息仍然需要可搜索，但是如果这些查询速度较慢也可以。）；delete（删除阶段，不再需要该索引，可以安全地将其删除）。 |
|minAge

|String

|30d

|否

|索引到达目标阶段所需要的时间。 |
|actions

|Struct

| |否

|当前阶段策略设置。详情请参见**actions字段数据结构说明**。 |

**actions字段数据结构说明**

|参数

|类型

|示例值

|是否必选

|描述 |
|----|----|-----|------|----|
|rollver

|Struct

| |否

|hot阶段的索引滚动更新操作。详情请参见**rollver字段数据结构说明**。 |
|setPriority

|Struct

| |否

|hot或cold阶段的索引优先级。详情请参见**setPriority字段数据结构说明**。 |
|allocate

|Struct

| |否

|cold阶段的分配操作。详情请参见**allocate字段数据结构说明**。 |
|delete

|Struct

|\{\}

|否

|删除索引操作。delete阶段开启时，属性必传，为空对象。 |

**rollver字段数据结构说明**

|参数

|类型

|示例值

|是否必选

|描述 |
|----|----|-----|------|----|
|maxAge

|String

|30d

|否

|触发滚动索引所需要的时间阈值。maxAge、maxDocs和maxSize三者中至少选填一个。单位：d（天）或者h（小时）。 |
|maxDocs

|Integer

|10000

|否

|触发滚动索引所需要的文档数量的阈值。 |
|maxSize

|String

|50gb

|否

|触发滚动索引所需要的索引大小的阈值。maxAge、maxDocs和maxSize三者中至少选填一个。单位：MB或者GB。 |

**setPriority字段数据结构说明**

|参数

|类型

|示例值

|是否必选

|描述 |
|----|----|-----|------|----|
|priority

|Integer

|100

|否

|当前节点默认的优先级。 |

**allocate字段数据结构说明**

|参数

|类型

|示例值

|是否必选

|描述 |
|----|----|-----|------|----|
|numberOfReplicas

|Integer

|1

|否

|分配指定的副本数，如果设置，则默认值为1。与migrate配合使用，开启自动迁移时，默认进行副本数分配。 |
|require

|Struct

| |否

|可选设置，冷热分离架构集群可用。详情请参见**require字段数据结构说明**。 |

**require字段数据结构说明**

|参数

|类型

|示例值

|是否必选

|描述 |
|----|----|-----|------|----|
|boxType

|String

|warm

|否

|自定义节点属性标识，冷热分离架构集群可用，迁移至冷节点。可选值：warm。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-nif24adwc0082****/ilm-policies/policy-1 HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "FF44681E-FD41-4FDE-B8DF-295DCDD6****",
    "Result": {
        "name": "policy-1",
        "phases": {
            "cold": {
                "minAge": "30d",
                "actions": {
                    "allocate": {
                        "numberOfReplicas": 1,
                        "require": {
                            "boxType": "warm"
                        }
                    },
                    "setPriority": {
                        "priority": 100
                    }
                }
            },
            "hot": {
                "minAge": "0s",
                "actions": {
                    "rollover": {
                        "maxAge": "30d",
                        "maxDocs": 10000,
                        "maxSize": "50gb"
                    },
                    "setPriority": {
                        "priority": 1000
                    }
                }
            },
            "delete": {
                "minAge": "30d",
                "actions": {
                    "delete": ""
                }
            }
        }
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

