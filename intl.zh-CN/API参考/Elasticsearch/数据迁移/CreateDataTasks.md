# CreateDataTasks

调用CreateDataTasks，创建索引迁移任务，将所选集群中的数据迁移到当前集群。

调用该接口前，需要注意：

-   目前一键索引迁移功能仅支持华北2（北京）区域。
-   源和目标端Elasticsearch集群需要满足：源端为6.7.0版本的自建或阿里云Elasticsearch集群，目标端为6.3.2或6.7.0版本的阿里云Elasticsearch集群。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=CreateDataTasks&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/instances/[InstanceId]/data-task HTTP/1.1
```

## 请求参数

|名称|类型|位置|是否必选|示例值|描述|
|--|--|--|----|---|--|
|ClientToken|String|Query|是|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|用于保证请求的幂等性。由客户端生成该参数值，要保证在不同请求间唯一，最大不超过64个ASCII字符。 |
|InstanceId|String|Path|是|es-cn-n6w1o1x0w001c\*\*\*\*|索引迁移的目标集群ID。 |

## RequestBody

RequestBody中还需填入以下参数，用来指定迁移信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|sourceCluster

|Struts

| | |源集群信息。 |
|└dataSourceType

|String

|是

|elasticsearch

|源集群类型。默认为elasticsearch。 |
|└endpoint

|String

|否

|http://yourdomain.com

|集群公网域名。源集群为开启公网时可填写。 |
|└vpcInstancePort

|Integer

|否

|9200

|访问集群的端口号。源集群采用VPC信息进行连接。 |
|└vpcId

|String

|否

|vpc-2ze59tt67m3nzkko9\*\*\*\*

|集群所在的专有网络ID。源集群采用VPC信息进行连接。 |
|└vpcInstanceId

|String

|否

|es-xxx-worker

|当前集群的实例ID或负载均衡SLB（Server Load Balancer）实例ID。源集群采用VPC信息进行连接。 |
|└vpcIp

|String

|否

|10.10.xx.xx

|集群中负载均衡实例的IP地址。源集群采用VPC信息进行连接 |
|└username

|String

|否

|elastic

|源集群的登录用户名。 |
|└password

|String

|否

|xxxxx

|源集群的登录密码。 |
|└index

|String

|是

|index\_001

|源集群的指定索引。 |
|└type

|String

|是

|index\_001

|指定索引的类型。 |
|sinkCluster

|Struts

| | |目标集群信息。 |
|└dataSourceType

|String

|是

|elasticsearch

|目标集群类型。 |
|└username

|String

|是

|elastic

|目标集群的登录用户名。 |
|└password

|String

|是

|xxxxx

|目标集群的登录密码。 |
|└index

|String

|是

|index\_001

|目标集群的指定索引。 |
|└type

|String

|是

|index\_001

|指定索引的类型。 |
|└settings

|String

|是

| |Settings配置。 |
|└mapping

|String

|是

| |Mapping配置。 |
|└routing

|String

|否

|id

|索引路由字段，默认使用主键字段。 |
|migrationConfig

|Struts

|否

| |迁移配置 |
|└filterParams

|String

|否

|index=111

|索引的过滤条件，过滤指定条件的文档来做索引重建。 |

**说明：** └表示子参数。

-   集群开启公网：填写endpoint参数进行连接。
-   集群未开启公网（或使用VPC信息连接集群）：填写参数vpcInstancePort、vpcId、vpcInstanceId或vpcInstancePort、vpcId、vpcIp进行连接。

示例如下：

-   公网访问集群

    ```
    
        {
            "sourceCluster":{
                "dataSourceType":"elasticsearch",
                "endpoint" : "http://es-cn-n6w1o1x0w001c****.public.elasticsearch.aliyuncs.com:9200",
                "username" : "elastic",
                "password" : "xxxxxx",
                "index" : "default",
                "type" : "default"
             },
            "sinkCluster":{
                "dataSourceType":"elasticsearch",
                "username" : "elastic",
                "password" : "xxxxxx",
                "index" : "default",
                "type" : "default",
                "settings" : "#settings 配置#",
                "mapping" : "#mapping配置#",
                "routing" : "_id"
            },
            "migrateConfig": {
                "sourceFilterParams": ""
            }
       }
         
    ```

-   阿里云Elasticsearch集群

    ```
    
        {
            "sourceCluster":{
                "dataSourceType" : "elasticsearch",
                "vpcInstancePort":9200,
                "vpcId":"vpc-2ze55voww95g82gak****",
                "vpcInstanceId":"es-cn-oew1oxiro000f****-worker",
                "username" : "elastic",
                "password" : "xxxxxx",
                "index" : "default",
                "type" : "default"
            },
            "sinkCluster":{
                "dataSourceType":"elasticsearch",
                "username" : "elastic",
                "password" : "xxxxxx",
                "index" : "default",
                "type" : "default",
                "settings" : "#settings 配置#",
                "mapping" : "#mapping配置#",
                "routing" : "_id"
            },
            "migrateConfig": {
                "sourceFilterParams": ""
            }
        }
        
    ```


## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Array of Result| |返回结果。 |
|sinkCluster|Struct| |目标集群信息。 |
|dataSourceType|String|elasticsearch|目标集群类型。 |
|index|String|index\_001|目标索引名。 |
|mapping|String|\{\\"doc\\":\{\\"properties\\":\{\\"interval\_ms\\":\{\\"type\\":\\"long\\"\},....\}|Mapping配置。 |
|password|String|xxxxx|目标集群的访问密码。 |
|routing|String|cluster\_name|路由字段，默认使用主键字段。 |
|settings|String|\{\\n \\"index\\": \{\\n \\"replication\\": \{\\n \\"type\\": .....\}|Settings配置。 |
|type|String|index\_001|目标索引类型。 |
|username|String|elastic|目标集群的用户名。 |
|vpcId|String|vpc-2ze55voww95g82gak\*\*\*\*|集群所在的专有网络ID（集群访问地址为公网域名可不填，私网地址需要填写）。 |
|vpcInstanceId|String|es-cn-oew1oxiro000f\*\*\*\*-worker|专有网络下集群的实例ID，或SLB实例ID。 |
|vpcInstancePort|String|9200|集群访问端口号。 |
|sourceCluster|Struct| |源集群信息。 |
|dataSourceType|String|elasticsearch|源集群类型，默认为elasticsearch。 |
|endpoint|String|http://10.20.xx.xx:9200|集群公网域名。 |
|index|String|index\_001|指定待迁移的索引。 |
|password|String|xxxxxx|源集群的访问密码。 |
|type|String|index\_001|指定的索引类型。 |
|username|String|elastic|源集群的用户名 |
|vpcId|String|vpc-2ze55voww95g82gak\*\*\*\*|源集群所在的专有网络ID（集群访问地址为公网域名可不填，私网地址需要填写）。 |
|vpcInstanceId|String|es-cn-oew1oxiro000f\*\*\*\*-worker|专有网络下集群的实例ID，或SLB实例ID。 |
|vpcInstancePort|Integer|9200|源集群的访问端口号。 |

## 示例

请求示例

```
POST /openapi/instances/es-cn-n6w1o1x0w001c****/data-task HTTP/1.1
公共请求头
{
    "sourceCluster":{
        "dataSourceType":"elasticsearch",
        "endpoint" : "http://es-cn-n6w1o1x0w001c****.public.elasticsearch.aliyuncs.com:9200",
        "username" : "elastic",
        "password" : "xxxxxx",
        "index" : "default",
        "type" : "default"
     },
    "sinkCluster":{
        "dataSourceType":"elasticsearch",
        "username" : "elastic",
        "password" : "xxxxxx",
        "index" : "default",
        "type" : "default",
        "settings" : "{\n  \"index\": {\n    \"replication\": {\n      \"type\": .....}",
        "mapping" : "{\"doc\":{\"properties\":{\"interval_ms\":{\"type\":\"long\"},....}",
        "routing" : "_id"
    },
    "migrateConfig":{
        "sourceFilterParams": "index = 1"
    }
  }
```

正常返回示例

`JSON`格式

```
{
    "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1****",
    "Result": [{
        "sourceCluster": {
            "password": "xxxxxx",
            "endpoint": "http://10.20.xx.xx:9200",
            "vpcId": "vpc-2ze55voww95g82gak****",
            "vpcInstancePort": 9200,
            "index": "index_001",
            "type": "index_001",
            "vpcInstanceId": "es-cn-oew1oxiro000f****-worker",
            "dataSourceType": "elasticsearch",
            "username": "elastic"
        },
        "sinkCluster": {
            "routing": "cluster_name",
            "settings": "{\\n  \\\"index\\\": {\\n    \\\"replication\\\": {\\n      \\\"type\\\": .....}",
            "password": "xxxxx",
            "mapping": "{\\\"doc\\\":{\\\"properties\\\":{\\\"interval_ms\\\":{\\\"type\\\":\\\"long\\\"},....}",
            "vpcInstancePort": 9200,
            "vpcId": "vpc-2ze55voww95g82gak****",
            "index": "index_001",
            "type": "index_001",
            "vpcInstanceId": "es-cn-oew1oxiro000f****-worker",
            "dataSourceType": "elasticsearch",
            "username": "elastic"
        }
    }]
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/elasticsearch)查看更多错误码。

