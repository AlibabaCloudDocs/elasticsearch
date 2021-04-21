# CreateDataTasks

Call the CreateDataTasks to create an index migration task to Data Transport from the selected cluster to the current cluster.

Before calling this interface, note:

-   Currently, the one-click index migration feature only supports the North China 2 \(Beijing\) region.
-   Source and target Elasticsearch clusters need to meet: self-built or Alibaba Cloud Elasticsearch Elasticsearch clusters with the source of version 6.7.0 and Alibaba Cloud Elasticsearch Elasticsearch clusters with the target of version 6.3.2 or 6.7.0.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=CreateDataTasks&type=ROA&version=2017-06-13)

## Request header

This operation uses common request headers, but does not use special request headers. For more information, see Common parameters.

## Request syntax

```

     POST /openapi/instances/[InstanceId]/data-task HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ClientToken|String|Query|Yes|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. You can use the client to generate the value, but you must ensure that it is unique among different requests. The token can contain only ASCII characters and cannot exceed 64 characters in length. |
|InstanceId|String|Path|Yes|es-cn-n6w1o1x0w001c\*\*\*\*|The ID of the destination cluster for index migration. |

## RequestBody

The following parameters must be filled in the RequestBody to specify the migration information.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|sourceCluster

|Struts

| | |The source cluster information. |
|└dataSourceType

|String

|Yes

|elasticsearch

|The source cluster type. The default is Elasticsearch. |
|└endpoint

|String

|No

|http://yourdomain.com

|The public network domain name of the cluster. The source cluster can be filled in when the public network is enabled. |
|└vpcInstancePort

|Integer

|No

|9200

|The port number to access the cluster. The source cluster uses VPC information to connect. |
|└vpcId

|String

|No

|vpc-2ze59tt67m3nzkko9\*\*\*\*

|The ID of the VPC where the source cluster resides. The source cluster uses VPC information to connect. |
|└vpcInstanceId

|String

|No

|es-xxx-worker

|The instance ID or Server Load Balancer \(SLB\) instance ID of the current cluster. The source cluster uses VPC information to connect. |
|└vpcIp

|String

|No

|10.10.xx.xx

|The IP address of the SLB instance in the cluster. Source clusters are connected using VPC information |
|└username

|String

|No

|elastic

|The logon of the source cluster. |
|└password

|String

|No

|xxxxx

|The logon password of the source cluster. |
|└index

|String

|Yes

|index\_001

|The specified index of the source cluster. |
|└type

|String

|Yes

|index\_001

|Specifies the type of the index. |
|sinkCluster

|Struts

| | |The target cluster information. |
|└dataSourceType

|String

|Yes

|elasticsearch

|The target cluster type. |
|└username

|String

|Yes

|elastic

|The logon of the target cluster. |
|└password

|String

|Yes

|xxxxx

|The logon password of the target cluster. |
|└index

|String

|Yes

|index\_001

|The specified index of the destination cluster. |
|└type

|String

|Yes

|index\_001

|Specifies the type of the index. |
|└settings

|String

|Yes

| |The Settings configuration. |
|└mapping

|String

|Yes

| |Mapping configuration. |
|└routing

|String

|No

|id

|The index routing field. The primary key field is used by default. |
|migrationConfig

|Struts

|No

| |Migration configurations |
|└filterParams

|String

|No

|index=111

|The filter condition of the index, which filters the documents of the specified condition for index reconstruction. |

**Note:** └ indicates a child parameter.

-   open public network for the cluster: enter the endpoint parameter to connect.
-   The cluster does not have public network \(or uses vpc information to connect to the cluster\): Fill in the parameter vpcInstancePort, vpcId, vpcInstanceId, or vpcInstancePort, vpcId, vpcIp to connect.

Example:

-   Public network access cluster

    ```
    
           {"sourceCluster":{ "dataSourceType":" Elasticsearch ", "endpoint" : "http://es-cn-n6w1o1x0w001c****.public. Elasticsearch .aliyuncs.com:9200", "username" : "elastic", "password" : "xxxxxx", "index" : "default", "type" : "default" }, "sinkCluster":{ "dataSourceType":" Elasticsearch ", " username ":" elastic ", " password ":" xxxxxx ", " index ":" default ", " type ":" default ", " settings ":"#settings configuration# ", " mapping ":"#mapping configuration# ", " routing ":"_id " }, " migrateConfig ": { "sourceFilterParams": "" } } 
         
    ```

-   Alibaba Cloud Elasticsearch cluster

    ```
    
           {"sourceCluster":{ "dataSourceType": "Elasticsearch ", "vpcInstancePort":9200, "vpcId":"vpc-2ze55voww95g82gak****", "vpcInstanceId":"es-cn-oew1oxiro000f****-worker", "username" : "elastic", "password" : "xxxxxx", "index" : "default", "type" : "default" }, "sinkCluster":{ "dataSourceType":" Elasticsearch", "username": "elastic", "password": "xxxxxx", "index": "default", "type": "default", "settings": "#settings configuration#", "mapping": "#mapping configuration#", "routing": "_id" }, "migrateConfig": { "sourceFilterParams": "" } } 
         
    ```


## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1\*\*\*\*|The ID of the request. |
|Result|Array of Result| |The returned results. |
|sinkCluster|Struct| |The target cluster information. |
|dataSourceType|String|elasticsearch|The target cluster type. |
|index|String|index\_001|The target index name. |
|mapping|String|\{\\"doc\\":\{\\"properties\\":\{\\"interval\_ms\\":\{\\"type\\":\\"long\\"\},....\}|Mapping configuration. |
|password|String|xxxxx|The access password of the target cluster. |
|routing|String|cluster\_name|The routing field. The primary key field is used by default. |
|settings|String|\{\\n \\"index\\": \{\\n \\"replication\\": \{\\n \\"type\\": .....\}|The Settings configuration. |
|type|String|index\_001|The destination index type. |
|username|String|elastic|The username of the target cluster. |
|vpcId|String|vpc-2ze55voww95g82gak\*\*\*\*|The Virtual Private Cloud ID where the cluster is located \(the access address of the cluster is a public domain name, but the private network address needs to be filled in\). |
|vpcInstanceId|String|es-cn-oew1oxiro000f\*\*\*\*-worker|The instance ID of the cluster under Virtual Private Cloud, or SLB instance ID. |
|vpcInstancePort|String|9200|The cluster access port number. |
|sourceCluster|Struct| |The source cluster information. |
|dataSourceType|String|elasticsearch|Source cluster type, which defaults to Elasticsearch. |
|endpoint|String|http://10.20.xx.xx:9200|The public network domain name of the cluster. |
|index|String|index\_001|Specify the indexes to be migrated. |
|password|String|xxxxxx|The access password of the source cluster. |
|type|String|index\_001|The specified index type. |
|username|String|elastic|the source cluster of user name |
|vpcId|String|vpc-2ze55voww95g82gak\*\*\*\*|The Virtual Private Cloud ID where the source cluster is located \(the cluster access address is a public network domain name, but the private network address needs to be filled in\). |
|vpcInstanceId|String|es-cn-oew1oxiro000f\*\*\*\*-worker|The instance ID of the cluster under Virtual Private Cloud, or SLB instance ID. |
|vpcInstancePort|Integer|9200|The access port number of the source cluster. |

## Examples

Sample requests

```

     POST /openapi/instances/es-cn-n6w1o1x0w001c ****/data-task HTTP/1.1 public request header {"sourceCluster":{ "dataSourceType": "Elasticsearch ", "endpoint" : "http://es-cn-n6w1o1x0w001c****.public. Elasticsearch .aliyuncs.com:9200", "username" : "elastic", "password" : "xxxxxx", "index" : "default", "type" : "default" }, "sinkCluster":{ "dataSourceType":" Elasticsearch ", "username" : "elastic", "password" : "xxxxxx", "index" : "default", "type" : "default", "settings" : "{\n \"index\": {\n \"replication\": {\n \"type\": .....}", "mapping" : "{\"doc\":{\"properties\":{\"interval_ms\":{\"type\":\"long\"},....}", "routing" : "_id" }, "migrateConfig":{ "sourceFilterParams": "index = 1" } } 
   
```

Sample success responses

`XML`format

```

     <RequestId>5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1****</RequestId> <Result> <sourceCluster> <password>xxxxxx</password> <endpoint>http://10.20.xx.xx:9200</endpoint> <vpcId>vpc-2ze55voww95g82gak****</vpcId> <vpcInstancePort>9200</vpcInstancePort> <index>index_001</index> <type>index_001</type> <vpcInstanceId>es-cn-oew1oxiro000f****-worker</vpcInstanceId> <dataSourceType>elasticsearch</dataSourceType> <username>elastic</username> </sourceCluster> <sinkCluster> <routing>cluster_name</routing> <settings>{\n \"index\": {\n \"replication\": {\n \"type\": .....}</settings> <password>xxxxx</password> <mapping>{\"doc\":{\"properties\":{\"interval_ms\":{\"type\":\"long\"},....}</mapping> <vpcInstancePort>9200</vpcInstancePort> <vpcId>vpc-2ze55voww95g82gak****</vpcId> <index>index_001</index> <type>index_001</type> <vpcInstanceId>es-cn-oew1oxiro000f****-worker</vpcInstanceId> <dataSourceType>elasticsearch</dataSourceType> <username>elastic</username> </sinkCluster> </Result> 
   
```

`JSON`Format

```

     { "RequestId": "5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1****", "Result": [{ "sourceCluster": { "password": "xxxxxx", "endpoint": "http://10.20.xx.xx:9200", "vpcId": "vpc-2ze55voww95g82gak****", "vpcInstancePort": 9200, "index": "index_001", "type": "index_001", "vpcInstanceId": "es-cn-oew1oxiro000f****-worker", "dataSourceType": "elasticsearch", "username": "elastic" }, "sinkCluster": { "routing": "cluster_name", "settings": "{\\n \\\"index\\\": {\\n \\\"replication\\\": {\\n \\\"type\\\": .....}", "password": "xxxxx", "mapping": "{\\\"doc\\\":{\\\"properties\\\":{\\\"interval_ms\\\":{\\\"type\\\":\\\"long\\\"},....}", "vpcInstancePort": 9200, "vpcId": "vpc-2ze55voww95g82gak****", "index": "index_001", "type": "index_001", "vpcInstanceId": "es-cn-oew1oxiro000f****-worker", "dataSourceType": "elasticsearch", "username": "elastic" } }] } 
   
```

## Error codes

Go to the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch) See more error codes.

