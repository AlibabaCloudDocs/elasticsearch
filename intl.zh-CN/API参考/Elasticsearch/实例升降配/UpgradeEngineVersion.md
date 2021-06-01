# UpgradeEngineVersion

调用UpgradeEngineVersion，升级Elasticsearch的实例版本或内核补丁版本。升级实例版本功能仅支持将6.3版本的实例升级至6.7版本。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=UpgradeEngineVersion&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/actions/upgrade-version HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|实例ID。 |
|clientToken|String|Query|否|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|dryRun|Boolean|Query|否|false|是否进行升级前校验。true表示校验，false表示不校验。 |
|version|String|Body|否|6.7|升级版本，可选值：6.7、ali1.2.0。取值为6.7时，type必须为engineVersion；取值为ali1.2.0时，type必须为aliVersion。 |
|type|String|Body|否|engineVersion|升级类型，可选值： engineVersion（版本升级，默认）、aliVersion（补丁升级）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6DC\*\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|status|String|success|校验是否通过。success表示通过，failed表示不通过。 |
|validateResult|Struct| |校验信息。 |
|errorCode|String|ClusterStatusNotHealth|错误码。 |
|errorMsg|String|The cluster status is not health|错误信息。 |
|errorType|String|clusterStatus|错误类型。支持：

 -   clusterStatus：集群健康状态
-   clusterConfigYml：集群YML文件
-   clusterConfigPlugins：集群配置文件
-   clusterResource：集群资源
-   clusterSnapshot：集群快照 |
|validateType|String|checkClusterHealth|校验类型。支持：

 -   checkClusterHealth：集群健康状态
-   checkConfigCompatible：配置兼容状态
-   checkClusterResource：资源空间状态
-   checkClusterSnapshot：是否存在快照 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/actions/upgrade-version HTTP/1.1
公共请求头
{
  "version": "ali1.2.0",
  "type": "aliVersion"
}
```

正常返回示例

`JSON`格式

```
{
    "Result":[
        {
            "validateType":"checkClusterHealth",
            "status":"failed",
            "validateResult":[
                {
                    "errorType":"clusterStatus",
                    "errorCode":"ClusterStatusNotHealth",
					"errorMsg":"ClusterStatusNotHealth"
                }
            ]
        },
        {
            "validateType":"checkConfigCompatible",
            "status":"failed",
            "validateResult":[
                {
                    "errorType":"clusterConfigYml",
                    "errorCode":"ClusterYamlNotCompatible",
					"errorMsg":"ClusterYamlNotCompatible"
                },
                {
                    "errorType":"clusterConfigPlugins",
                    "errorCode":"ClusterPluginsNotSupport",
					"errorMsg":"ClusterPluginsNotSupport"
                }
            ]
        },
        {
            "validateType":"checkClusterResource",
            "status":"failed",
            "validateResult":[
                {
                    "errorType":"clusterResource",
                    "errorCode":"ClusterResourceNotEnough",
					"errorMsg":"ClusterResourceNotEnough"
                }
            ]
        },
        {
            "validateType":"checkClusterSnapshot",
            "status":"failed",
            "validateResult":[
                {
                    "errorType":"clusterSnapshot",
                    "errorCode":"ClusterSnapshotNotAvaild",
					"errorMsg":"ClusterSnapshotNotAvaild"
                }
            ]
        }
    ],
"RequestId":"F99407AB-2FA9-489E-A259-40CF6DC****"
}
```

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|400|InstanceActivating|Instance is activating.|实例目前处于生效中。|
|400|InstanceNotFound|The instanceId provided does not exist.|实例找不到，请核对实例状态。|

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

