# ListVpcEndpoints

调用ListVpcEndpoints，用于查看服务vpc下的终端节点状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListVpcEndpoints&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/vpc-endpoints HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-2r429tctl000d\*\*\*\*|实例ID。 |
|size|Integer|Query|否|10|分页查询时设置的每页条数。默认值：20。 |
|page|Integer|Query|否|1|列表的页码。

 起始值：1，默认值：1。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC47D9|请求ID。 |
|Result|Array of Result| |终端节点信息详情。 |
|connectionStatus|String|Disconnected|终端节点连接状态，取值含义如下：

 -   Pending：修改中。
-   Connecting：连接中。
-   Connected：已连接。
-   Disconnecting：断开连接中。
-   Disconnected：未连接。
-   Deleting：删除中。
-   ServiceDeleted：终端节点对应的服务已删除。 |
|createTime|String|2021-07-22T01:19:24Z|终端节点的创建时间。 |
|endpointBusinessStatus|String|Normal|终端节点的业务状态，取值含义如下：

 -   Normal：正常。
-   FinacialLocked：欠费锁定。 |
|endpointDomain|String|ep-bp18s6wy9420wdi4\*\*\*\*.epsrv-bp1bz3efowa4kc0\*\*\*\*.cn-hangzhou.privatelink.aliyuncs.com|终端节点域名，用于连接配置。 |
|endpointId|String|ep-bp1tah7zbrwmkjef\*\*\*\*|终端节点ID。 |
|endpointName|String|test|终端节点名称。 |
|endpointStatus|String|Active|终端节点状态，取值含义如下：

 -   Creating：创建中。
-   Active：可用。
-   Pending：修改中。
-   Deleting：删除中。 |
|serviceId|String|epsrv-bp1w0p3jdirbfmt6\*\*\*\*|终端节点关联的终端节点服务的ID。 |
|serviceName|String|com.aliyuncs.privatelink.cn-hangzhou.epsrv-bp1w0p3jdirbfmt6\*\*\*\*|终端节点关联的终端节点服务的名称。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-2r429tctl000d****/vpc-endpoints HTTP/1.1
```

正常返回示例

`JSON`格式

```
{
    "Result": [
        {
            "endpointId": "ep-bp18s6wy9420wdi4****",
            "serviceId": "epsrv-bp1bz3efowa4kc03****",
            "zoneId": "cn-hangzhou-i",
            "securityGroupId": "sg-bp1emvef6hl69pqs****",
            "createTime": "2021-07-22T01:19:24Z",
            "endpointStatus": "Active",
            "endpointBusinessStatus": "Normal",
            "endpointDomain": "ep-bp18s6wy9420wdi4****.epsrv-bp1bz3efowa4kc0****.cn-hangzhou.privatelink.aliyuncs.com",
            "connectionStatus": "Disconnected",
            "serviceName": "com.aliyuncs.privatelink.cn-hangzhou.epsrv-bp1bz3efowa4kc03****"
        },
        {
            "endpointId": "ep-bp1tah7zbrwmkjef****",
            "serviceId": "epsrv-bp1w0p3jdirbfmt6****",
            "zoneId": "cn-hangzhou-i",
            "securityGroupId": "sg-bp1emvef6hl69pqs****",
            "createTime": "2021-07-22T01:12:28Z",
            "endpointStatus": "Active",
            "endpointBusinessStatus": "Normal",
            "endpointDomain": "ep-bp1tah7zbrwmkjef****.epsrv-bp1w0p3jdirbfmt6****.cn-hangzhou.privatelink.aliyuncs.com",
            "connectionStatus": "Connected",
            "serviceName": "com.aliyuncs.privatelink.cn-hangzhou.epsrv-bp1w0p3jdirbfmt6****"
        }
    ],
    "RequestId": "AD080217-B2C1-4F36-904F-BE4E4A0B4536"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

