# ListDataTasks

调用ListDataTasks，获取数据迁移任务信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListDataTasks&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/data-task HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-oew1oxiro000f\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|createTime|String|2020-07-30 06:32:18|任务创建的时间。 |
|sinkCluster|Struct| |目标集群信息。 |
|dataSourceType|String|1|目标集群类型。默认为elasticsearch。 |
|endpoint|String|http://192.168.xx.xx:4101|目标集群的公网访问地址。 |
|index|String|product\_info|目标索引。 |
|type|String|\_doc|索引类型。 |
|vpcId|String|vpc-2ze55voww95g82gak\*\*\*\*|集群所在的专有网络ID。 |
|vpcInstanceId|String|es-cn-09k1rnu3g0002\*\*\*\*-worker|当前集群的实例ID或负载均衡SLB（Server Load Balancer）实例ID。 |
|vpcInstancePort|String|9200|集群的访问端口号。 |
|sourceCluster|Struct| |源集群信息。 |
|dataSourceType|String|1|源集群类型。默认为elasticsearch。 |
|index|String|product\_info|待迁移的索引。 |
|mapping|String|\{\\"\_doc\\":\{\\"properties\\":\{\\"user\\":\{\\"properties\\":\{\\"last\\":\{\\"type\\":\\"text\\",...\}\}\}\}\}\}|集群的Mapping配置。 |
|routing|String|\_id|索引路由字段，默认使用主键字段。 |
|settings|String|\{\\n \\"index\\": \{\\n \\"replication\\": \{\\n\}.....\}\}|集群的Settings配置。 |
|type|String|\_doc|索引类型。 |
|status|String|SUCCESS|任务状态。 |
|taskId|String|et\_cn\_mfv1233r47272\*\*\*\*|任务ID。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-oew1oxiro000f****/data-task HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

