# GetClusterDataInformation

调用GetClusterDataInformation，获取集群的数据信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=elasticsearch&api=GetClusterDataInformation&type=ROA&version=2017-06-13)

## 请求头

该接口使用公共请求头，无特殊请求头。请参见公共请求参数文档。

## 请求语法

```
POST /openapi/cluster/data-information HTTP/1.1
```

## 请求参数

请求参数为空，但需填写RequestBody。

RequestBody中需要填入以下参数，用来指定集群信息。

|名称

|类型

|是否必选

|示例值

|描述 |
|----|----|------|-----|----|
|dataSourceType

|String

|是

|elasticsearch

|集群类型，默认为elasticsearch。 |
|endpoint

|String

|否

|http://10.01.xx.xx

|集群公网域名。源集群的网络环境为公网时必填。 |
|instanceId

|String

|否

|es-cn-09k1rnu3g0002\*\*\*\*

|Elasticsearch实例ID。源集群的网络环境为阿里云Elasticsearch集群时必填。 |
|vpcInstancePort

|Integer

|否

|9200

|集群的访问端口号。源集群的网络环境为阿里云Elasticsearch集群、阿里云ECS服务自建集群时必填。 |
|vpcId

|String

|否

|vpc-2ze59tt67m3nzkko9\*\*\*\*

|集群所在的专有网络ID。源集群的网络环境为阿里云Elasticsearch集群、阿里云ECS服务自建集群时必填。 |
|vpcInstanceId

|String

|否

|es-09k1rnu3g0002\*\*\*\*-worker

|当前集群的实例ID或负载均衡SLB（Server Load Balancer）实例ID。源集群的网络环境为阿里云Elasticsearch集群时必填。 |
|username

|String

|否

|elastic

|集群的访问用户名。 |
|password

|String

|否

|xxxxxx

|集群的访问密码。 |
|index

|String

|否

|product

|索引名称。 |
|type

|String

|否

|default

|索引类型。 |

**说明：**

-   index为空，可校验Elasticsearch集群是否可连通。如果可连通，返回结果中indices不为空。
-   index不为空，可获取当前index下，settings和mapping的信息。
-   type不为空，可获取当前type对应的routing字段信息。

源集群的网络环境不同，需要填写的参数不同：

-   公网集群：需要填写的参数包括endpoint，示例如下。

    ```
    
            {
             "dataSourceType": "elasticsearch",
             "endpoint": "http://es-cn-npk1shyiq000d****.public.elasticsearch.aliyuncs.com:9200",
             "username": "elastic",
             "password": "xxxxxx",
             "index": "default",
             "type": "default"
             }
          
    ```

-   阿里云ECS服务自建集群：需要填写的参数包括vpcInstancePort、vpcId和vpcIp，示例如下。

    ```
    
           {
                "dataSourceType": "elasticsearch",
                "vpcId":"vpc-2ze55voww95g82gak****",
                "vpcInstancePort": "9200",
                "vpcIp": "10.12.xx.xx",
                "username" : "elastic",
                "password" : "xxxxxx",
                "index":  "product",
                "type" : "default"
           }
          
    ```

-   阿里云Elasticsearch集群：需要填写的参数包括vpcInstancePort、vpcId、vpcInstanceId和instanceId，示例如下。

    ```
    
        {
            "dataSourceType" : "elasticsearch",
            "vpcId":"vpc-2ze55voww95g82gak****",
            "vpcInstancePort" : "9200",
            "vpcInstanceId" : "es-cn-09k1rnu3g0002****-worker",
            "instanceId" : "es-cn-oew1oxiro000f****",
            "username" : "elastic",
            "password" : "xxxxxx",
            "index":  "product",
            "type" : "default"
         }
    
        
    ```


## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|请求ID。 |
|Result|Struct| |返回结果。 |
|connectable|Boolean|true|是否可连通。 |
|metaInfo|Struct| |集群的元数据信息。 |
|fields|List|\["id","name"\]|索引Mapping对应的字段。 |
|indices|List|\["index1","index2","index3"\]|集群的索引列表。 |
|mapping|String|\{\\"\_doc\\":\{\\"properties\\":\{\\"user\\":\{\\"properties\\":\{\\"last\\":\{\\"type\\":\\"text\\",...\}\}\}\}\}\}|集群的Mapping配置。 |
|settings|String|\{\\n \\"index\\": \{\\n \\"replication\\": \{\\n\}.....\}\}|集群的Settings配置。 |
|typeName|List|\["index1-type"\]|指定索引的type。 |

## 示例

请求示例

```
POST /openapi/cluster/data-information HTTP/1.1
公共请求头
 {
     "dataSourceType": "elasticsearch",
     "endpoint": "http://es-cn-npk1shyiq000d****.public.elasticsearch.aliyuncs.com:9200",
     "username": "elastic",
     "password": "xxxxxx",
     "index": "default",
     "type": "default"
}
```

正常返回示例

`JSON`格式

```
{
  "Result": {
    "connectable": true,
    "metaInfo": {
      "indices":  ["index1","index2","index3"],
      "typeName": ["index1-type"], 
      "settings": "{\n  \"index\": {\n    \"replication\": {\n}.....}}",  
      "mapping": "{\"_doc\":{\"properties\":{\"user\":{\"properties\":{\"last\":{\"type\":\"text\",...}}}}}}", 
      "fields": ["id","name","_id"]
    }
  },
  "RequestId" : "29AEFBA7-DD86-4B05-87A2-43F22C85****"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/elasticsearch)查看更多错误码。

