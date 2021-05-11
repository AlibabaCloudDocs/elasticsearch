# GetClusterDataInformation

Call GetClusterDataInformation to obtain the data information about the cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=GetClusterDataInformation&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
POST /openapi/cluster/data-information HTTPS|HTTP
```

## Request parameters

The request parameters is empty, but the RequestBody parameter is required.

You must specify the cluster information by setting the following parameters in RequestBody:

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|dataSourceType

|String

|Yes

|elasticsearch

|The type of the cluster. Default value: elasticsearch. |
|endpoint

|String

|No

|http://10.01.xx.xx

|The public domain name of the cluster. This parameter must be specified when the network environment of the source cluster is a public network. |
|instanceId

|String

|No

|es-cn-09k1rnu3g0002\*\*\*\*

|The ID of the Elasticsearch instance. This parameter is required when the network environment of the source cluster is an Alibaba Cloud Elasticsearch cluster. |
|vpcInstancePort

|Integer

|No

|9200

|The access port number of the cluster. The source cluster network environment is required when the Alibaba Cloud Elasticsearch cluster or Alibaba Cloud ECS Service User-created cluster is used. |
|vpcId

|String

|No

|vpc-2ze59tt67m3nzkko9\*\*\*\*

|The ID of the VPC to which the cluster belongs. The source cluster network environment is required when the Alibaba Cloud Elasticsearch cluster or Alibaba Cloud ECS Service User-created cluster is used. |
|vpcInstanceId

|String

|No

|es-09k1rnu3g0002\*\*\*\*-worker

|The instance ID or Server Load Balancer \(SLB\) ID of the current cluster. This parameter is required when the network environment of the source cluster is an Alibaba Cloud Elasticsearch cluster. |
|username

|String

|No

|elastic

|The username that is used to connect to the cluster. |
|password

|String

|No

|xxxxxx

|The password that is used to connect to the cluster. |
|index

|String

|No

|product

|The name of the destination index. |
|type

|String

|No

|default

|The type of the destination index. |

**Note:**

-   If the index is empty, you can check whether the Elasticsearch cluster can be connected. If the connectivity is available, the indices field in the returned results is not null.
-   If the index is not empty, you can get the information about the settings and mappings under the current index.
-   If the type is not empty, you can obtain the routing field of the current type.

The parameters vary depending on the network environment of the source cluster.

-   Public network cluster: enter one or more parameters, including endpoint. Example:

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

-   Alibaba Cloud ECS Service self-built cluster: Enter the following parameters: vpcInstancePort, vpcId, and vpcIp. For example:

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

-   Alibaba Cloud Elasticsearch cluster: specify the parameters: vpcInstancePort, vpcId, vpcInstanceId, and instanceId. The sample code is as follows:

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


## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Struct| |The return results. |
|connectable|Boolean|true|Whether it is connectable. |
|metaInfo|Struct| |The metadata of the cluster. |
|fields|List|\["id","name"\]|The fields in the Mapping for the index. |
|indices|List|\["index1","index2","index3"\]|The index list of the cluster. |
|mapping|String|\{\\"\_doc\\":\{\\"properties\\":\{\\"user\\":\{\\"properties\\":\{\\"last\\":\{\\"type\\":\\"text\\",...\}\}\}\}\}\}|The Mapping configuration of the cluster. |
|settings|String|\{\\n \\"index\\": \{\\n \\"replication\\": \{\\n\}.....\}\}|The Settings of the cluster. |
|typeName|List|\["index1-type"\]|Specifies the type of the index. |

## Examples

Sample requests

```
POST /openapi/cluster/data-information HTTP/1.1
Common request parameters
 {
     "dataSourceType": "elasticsearch",
     "endpoint": "http://es-cn-npk1shyiq000d****.public.elasticsearch.aliyuncs.com:9200",
     "username": "elastic",
     "password": "xxxxxx",
     "index": "default",
     "type": "default"
}
```

Sample success responses

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

