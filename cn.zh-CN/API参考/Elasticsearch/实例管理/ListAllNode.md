# ListAllNode

调用ListAllNode，获取集群下的所有节点信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=ListAllNode&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
GET /openapi/instances/[InstanceId]/nodes HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DCC\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|cpuPercent|String|4.1%|CPU使用率。 |
|diskUsedPercent|String|2.0%|磁盘使用率。 |
|health|String|GREEN|节点健康状态。支持：GREEN、YELLOW、RED和GRAY。 |
|heapPercent|String|47.9%|JVM内存。 |
|host|String|10.6.15.32|节点IP。 |
|loadOneM|String|0.09|一分钟负载。 |
|nodeType|String|WORKER|节点类型，可选值：

 -   MASTER：专有主节点
-   WORKER：热节点
-   WORKER\_WARM：冷节点
-   COORDINATING：协调节点
-   KIBANA：Kibana节点 |
|port|Integer|9200|节点访问端口。 |
|zoneId|String|cn-hangzhou-g|节点所在可用区。 |

## 示例

请求示例

```
GET /openapi/instances/es-cn-n6w1o1x0w001c****/nodes HTTP/1.1
公共请求头
```

正常返回示例

`JSON`格式

```
{
	"Result": [
		{
			"nodeType": "WORKER_WARM",
			"host": "172.16.xx.xx",
			"port": 9200,
			"zoneId": "cn-hangzhou-i",
			"heapPercent": "70.8%",
			"cpuPercent": "4.1%",
			"loadOneM": "0.09",
			"diskUsedPercent": "2.0%",
			"health": "GREEN"
		},
		{
			"nodeType": "WORKER_WARM",
			"host": "172.16.xx.xx",
			"port": 9200,
			"zoneId": "cn-hangzhou-i",
			"heapPercent": "45.0%",
			"cpuPercent": "3.3%",
			"loadOneM": "0.00",
			"diskUsedPercent": "3.0%",
			"health": "GREEN"
		},
		{
			"nodeType": "WORKER",
			"host": "172.16.xx.xx",
			"port": 9200,
			"zoneId": "cn-hangzhou-i",
			"heapPercent": "53.4%",
			"cpuPercent": "3.7%",
			"loadOneM": "0.02",
			"diskUsedPercent": "1.0%",
			"health": "GREEN"
		}
	],
	"RequestId": "76387A8A-5ED9-43B4-AC90-76146209****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

