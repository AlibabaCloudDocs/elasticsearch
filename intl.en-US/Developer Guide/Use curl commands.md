# Use curl commands

This topic describes how to use curl commands to access an Alibaba Cloud Elasticsearch cluster, query cluster information, create indexes and documents, and search for documents.

An Elastic Compute Service \(ECS\) instance is created in the same Virtual Private Cloud \(VPC\) as your Elasticsearch cluster. For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

**Note:** You can also use an existing ECS instance that resides in the same VPC as your Elasticsearch cluster. For more information about how to use an ECS instance that resides in the classic network to access an Elasticsearch cluster that resides in a VPC, see [Access to an Alibaba Cloud Elasticsearch cluster from the classic network](/intl.en-US/Elasticsearch Instances Management/FAQ/Access to an Alibaba Cloud Elasticsearch cluster from the classic network.md).

Open source Elasticsearch provides a series of RESTful API operations that you can use by running curl commands or in the Kibana console. You can refer to this topic to call RESTful API operations by running curl commands. For more information about how to use RESTful API operations in the Kibana console, see [Quick start](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 3: Access a cluster.md).

**Note:** In the sample code across this topic, replace `es_password` with the password of your Elasticsearch cluster.

## Run a curl command to access your Elasticsearch cluster

1.  Connect to the ECS instance.

    For more information, see [Step 3: Connect to the ECS instance](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Linux instances.md).

2.  Run the following curl command to access your Elasticsearch cluster.

    **Note:** If the system displays `curl command not found`, you must run the `yum install curl` command to install cURL on the ECS instance.

    ```
    curl -u <username>:<password> http://<host>:<port>
    ```

    |Variable|Description|
    |--------|-----------|
    |<username\>|The username that is used to access your Elasticsearch cluster. We recommend that you use a username other than **elastic**. **Note:**

    -   If you use the **elastic** account to access your Elasticsearch cluster and then reset the password of the account, it may require some time for the new password to take effect. During this period, you cannot use the **elastic** account to access the cluster. Therefore, we recommend that you do not use the **elastic** account to access an Elasticsearch cluster.
    -   If the version of your Elasticsearch cluster contains with\_X-Pack, you must specify both the username and password to access the Elasticsearch cluster. |
    |<password\>|The password that is used to access your Elasticsearch cluster. Enter the password that is specified when you create the Elasticsearch cluster or initialize Kibana.|
    |<host\>|The **internal endpoint** of your Elasticsearch cluster. You can obtain the internal endpoint from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the Elasticsearch cluster.|
    |<port\>|The port of your Elasticsearch cluster. The default port is **9200**. You can obtain the port number from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the Elasticsearch cluster.|

    Example command:

    ```
    curl -u elastic:es_password http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200
    ```

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Successful response](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1224649951/p58858.png)


## Run a curl command to query cluster information

-   Query the health status of the cluster

    ```
    curl -u elastic:es_password -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_cat/health?v'
    ```

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Query the health status of the cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88445.png)

-   Query indexes in the cluster

    ```
    curl -u elastic:es_password -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_cat/indices?v'
    ```

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Query indexes in the cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88448.png)


## Run a curl command to create an index or a document

-   Create an index

    ```
    curl -u elastic:es_password -XPUT 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info'
    ```

    In the preceding example, an index named `product_info` is created.

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Create an index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88449.png)

-   Set a mapping for an index

    ```
    curl -u elastic:es_password -XPUT 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/_mapping' -H 'Content-Type: application/json' -d '
    {
     "_doc":{
       "properties": {
            "productName": {"type": "text","analyzer": "ik_smart"},
            "annual_rate":{"type":"keyword"},
            "describe": {"type": "text","analyzer": "ik_smart"}
          }
      }
    }'
    ```

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Set a mapping for an index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88464.png)

    In the preceding example, the type of the `product_info` index is set to `_doc`. The index contains the `productName`, `annual_rate`, and `describe` fields. This example also defines a tokenizer for the fields.

-   Create a document and insert data

    ```
    curl -u elastic:es_password -XPOST 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/1?pretty' -H 'Content-Type: application/json' -d '
    {
    "productName":"testpro",
    "annual_rate":"3.22%",
    "describe":"testpro"
    }'
    ```

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Create a document and insert data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88456.png)

    In the preceding example, a document named `1` is created in the `product_info` index of the `_doc` type, and a data record is inserted into the document.


## Run a curl command to search for a document

```
curl -u elastic:es_password -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/1?pretty'
```

If the command is successfully executed, the result shown in the following figure is returned.

![Search for a document](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88461.png)

In the preceding example, a document named `1` is searched for.

## Run a curl command to delete an index

```
curl -u elastic:es_password -XDELETE 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info'
```

If the command is successfully executed, the result shown in the following figure is returned.

![Delete an index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88462.png)

In the preceding example, an index named `product_info` is deleted.

**Note:** For more information about curl commands, see [open source Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html).

