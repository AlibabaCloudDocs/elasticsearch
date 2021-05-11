# MigrateToOtherZone

调用MigrateToOtherZone，迁移对应可用区下的节点到目标可用区。

当您升配实例，遇到可用区规格库存不足的问题时，可以通过迁移可用区节点来解决。在调用此接口前，需要确保：

-   当前账号存在资源充足的可用区。

    在迁移当前规格的节点到其他可用区后，需手动[升配集群](~~96650~~)，并不会在迁移过程中升配集群，因此要选择资源充足的可用区，避免集群升配失败。建议优先选择字母顺序靠后的新可用区（例如对于cn-hangzhou-e和cn-hangzhou-h可用区，优先选择cn-hangzhou-h）。

-   集群处于健康状态。

    可通过`GET _cat/health?v`命令查看集群健康状态。


## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=MigrateToOtherZone&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/migrate-zones HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|dryRun|Boolean|Query|是|false|校验是否可以进行可用区节点迁移。true表示只校验不执行迁移任务；false表示校验通过后即进行迁移任务。 |
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定迁移的可用区信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|fromZoneId

|String

|是

|cn-hangzhou-i

|实例目前所在的可用区。 |
|toZoneId

|String

|是

|cn-hangzhou-b

|迁移到的目标可用区。 |
|toVswitchId

|String

|是

|vsw-bp1f7r0ma00pf9h2l\*\*\*\*

|虚拟交换机ID。 |

示例如下。

```

{
    "fromZoneId": "cn-hangzhou-e",
    "toZoneId": "cn-hangzhou-f",
    "toVswitchId": "vsw-bp16t5hpc689dgkgc****"
}

```

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：迁移成功
-   false：迁移失败 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/migrate-zones?dryRun=false HTTP/1.1
公共请求头
{
    "fromZoneId": "cn-hangzhou-e",
    "toZoneId": "cn-hangzhou-f",
    "toVswitchId": "vsw-bp16t5hpc689dgkgc****"
}
```

正常返回示例

`JSON`格式

```
{
	"Result": true,
	"RequestId": "24A77388-9444-49A3-A1CF-F48385E5****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

