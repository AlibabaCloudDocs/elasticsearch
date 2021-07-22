# ModifyWhiteIps

调用ModifyWhiteIps，更新指定实例的访问白名单。

**说明：** 调用该接口时，您需要注意：当实例状态为生效中（activating）、失效（invalid）和冻结（inactive）时，系统无法更新信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ModifyWhiteIps&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
PATCH|POST /openapi/instances/[InstanceId]/actions/modify-white-ips HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-0pp1jxvcl000z\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|nodeType|String|Body|否|KIBANA|节点类型。可选值：WORKER（Elasticsearch集群）、KIBANA（Kibana集群）。如果选填了whiteIpList参数，则该参数必填。 |
|networkType|String|Body|否|PRIVATE|网络类型。可选值：PRIVATE（私网）、PUBLIC（公网）。如果选填了whiteIpList参数，则该参数必填。 |
|modifyMode|String|Body|否|Cover|修改方式，取值含义如下：

 -   Cover（默认值）：使用ips参数的值覆盖原IP白名单。
-   Append：在原IP白名单中增加ips参数中输入的IP地址。
-   Delete：Delete：在原IP白名单中删除ips参数中输入的IP地址，至少需要保留一个IP地址。 |
|whiteIpList|List<String\>|Body|否|\["0.0.0.0/0","0.0.0.0/1"\]|白名单列表。 |
|whiteIpGroup|Struct|Body|否|\{ "groupName": "test\_group\_name", "ips": \[ "0.0.0.0", "10.2.XX.XX" \] \}|以白名单组whiteIpGroup传参方式，更新实例白名单安全配置。仅支持更新一个白名单组。 |
|groupName|String| |否|test\_group\_name|白名单组的组名。如果选填了whiteIpGroup参数，则该参数必填。 |
|ips|String| |否|\["0.0.0.0", "10.2.XX.XX"\]|白名单组中的IP列表。如果选填了whiteIpGroup参数，则该参数必填。 |

**说明：**

modifyMode为Cover时，如果ips为空，则删除该白名单组。

modifyMode为Cover时，如果groupName不在已有白名单组组名的列表中，则会新建一个白名单组。

modifyMode为Delete时，删除后的ips至少需要保留一个IP地址。

modifyMode为Append时，需要保证白名单组组名为已创建的，否则会报NotFound。

白名单组的增加、删除，是由modifyMode为Cover的调用来实现的，Delete和Append无法实现白名单组粒度的增删，只能修改白名单组中的IP列表。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*\*|请求ID。 |
|Result|Boolean|true|返回结果：

 -   true：白名单更新成功。
-   false：白名单更新失败。 |

以下返回的参数信息中我们只保证包含上面返回参数表格中提到的这些参数信息，而未提到的这些参数信息仅供参考，程序中不能强制依赖获取这些参数。

## 示例

请求示例

```
PATCH /openapi/instances/es-cn-0pp1jxvcl000z****/actions/modify-white-ips HTTP/1.1

{
  "nodeType":"KIBANA",
  "networkType":"PRIVATE",
  "whiteIpList": ["0.0.0.0/0","0.0.0.0/1"]
}

或

{
  	"nodeType":"KIBANA",
  	"networkType":"PRIVATE",
		"whiteIpGroup": {
  			"groupName": "test_group_name",
    		"ips":	[
    				"0.0.0.0",
      			"10.2.XX.XX"
    		]
  	}
}
```

正常返回示例

`JSON`格式

```
{
    "Result": true,
    "RequestId": "4EB052CA-17A1-4D45-A932-E1F08C7964CB"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

