# Use API operations to manage an Alibaba Cloud Elasticsearch cluster

Open source Elasticsearch provides a series of RESTful API operations that you can call by running curl commands or by using the Kibana console. This topic describes how to use curl commands and call API operations to access an Alibaba Cloud Elasticsearch cluster, query cluster information, create indexes and documents, and search for documents.

An Elastic Compute Service \(ECS\) instance is created in the virtual private cloud \(VPC\) where your Alibaba Cloud Elasticsearch cluster resides. For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

**Note:** You can also use an existing ECS instance that resides in the same VPC as your Elasticsearch cluster. For more information about how to use an ECS instance that resides in the classic network to access an Elasticsearch cluster that resides in a VPC, see [Access to an Alibaba Cloud Elasticsearch cluster from the classic network]().

You can refer to this topic to call API operations by running curl commands. For more information about how to call the API operations by using the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Access your Elasticsearch cluster

1.  Connect to the ECS instance.

    For more information, see [Step 3: Connect to the ECS instance](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Linux instances.md).

2.  Run the following command to access your Elasticsearch cluster:

    **Note:** If the system displays `curl command not found`, you must run the `yum install curl` command to install cURL on the ECS instance.

    ```
    curl -u <user>:<password> http://<host>:<port>
    ```

    |Parameter|Description|
    |---------|-----------|
    |<user\>|The username that is used to access your Elasticsearch cluster. We recommend that you use a username other than elastic.**Note:**

    -   If you use the elastic account to access your Elasticsearch cluster and then reset the password of the account, it may require some time for the new password to take effect. During this period, you cannot use the elastic account to access the cluster. Therefore, we recommend that you do not use the elastic account to access an Elasticsearch cluster. You can log on to the Kibana console and create a user with the required role to access the Elasticsearch cluster. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md).
    -   If the version of your Elasticsearch cluster contains with\_X-Pack, you must specify both the username and password to access the Elasticsearch cluster. |
    |<password\>|The password that is used to access your Elasticsearch cluster. The password of the elastic account is specified when you create the Elasticsearch cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
    |<host\>|The internal endpoint of your Elasticsearch cluster. You can obtain the internal endpoint from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the Elasticsearch cluster.|
    |<port\>|The port number that is used to access your Elasticsearch cluster. The default port number is **9200**. You can obtain the port number from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the Elasticsearch cluster.|

    Example command:

    ```
    curl -u <user>:<password> http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200
    ```

    If the running of the command is successful, the result shown in the following figure is returned.

    ![Successful response](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1224649951/p58858.png)


## Query the information of the cluster

-   Query the health status of the cluster

    ```
    curl -u <user>:<password> -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_cat/health?v'
    ```

    If the running of the command is successful, the result shown in the following figure is returned.

    ![Query the health status of the cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88445.png)

-   Query indexes in the cluster

    ```
    curl -u <user>:<password> -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_cat/indices?v'
    ```

    If the running of the command is successful, the result shown in the following figure is returned.

    ![Query indexes in the cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88448.png)


## Create indexes and documents

-   Create an index

    ```
    curl -u <user>:<password> -XPUT 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info'
    ```

    In the preceding example, an index named `product_info` is created.

    If the running of the command is successful, the result shown in the following figure is returned.

    ![Create an index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88449.png)

-   Configure a mapping for an index

    ```
    curl -u <user>:<password> -XPUT 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/_mapping' -H 'Content-Type: application/json' -d '
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

    If the running of the command is successful, the result shown in the following figure is returned.

    ![Configure a mapping for an index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88464.png)

    In the preceding example, the type of the `product_info` index is set to `_doc`. The index contains the `productName`, `annual_rate`, and `describe` fields. This example also defines tokenizers for the fields.

-   Create documents and insert data
    -   Create a single document

        ```
        curl -u <user>:<password> -XPOST 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/1?pretty' -H 'Content-Type: application/json' -d '
        {
        "productName":"testpro",
        "annual_rate":"3.22%",
        "describe":"testpro"
        }'
        ```

        If the running of the command is successful, the result shown in the following figure is returned.

        ![Create a single document and insert data into the document](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88456.png)

        In the preceding example, a document named `1` is created in the `product_info` index of the `_doc` type, and a data record is inserted into the document.

    -   Create multiple documents

        ```
        curl -u <user>:<password> -XPOST http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_bulk -H 'Content-Type: application/json' -d'
        { "index" : { "_index": "product_info", "_type" : "_doc", "_id" : "1" } }
        {"productName":"testpro","annual_rate":"3.22%","describe":"testpro"}
        { "index" : { "_index": "product_info", "_type" : "_doc", "_id" : "2" } }
        {"productName":"testpro1","annual_rate":"3.26%","describe":"testpro"}'
        ```

        If the running of the command is successful, the result shown in the following figure is returned.

        ![Create multiple documents](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1716522161/p212941.png)

        In the preceding example, documents named `1` and `2` are created in the `product_info` index of the `_doc` type, and a data record is inserted into each of the documents.


## Search for a document

```
curl -u <user>:<password> -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/1?pretty'
```

If the running of the command is successful, the result shown in the following figure is returned.

![Search for a document](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88461.png)

In the preceding example, a document named `1` is searched for.

## Delete an index

```
curl -u <user>:<password> -XDELETE 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info'
```

If the running of the command is successful, the result shown in the following figure is returned.

![Delete an index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2224649951/p88462.png)

In the preceding example, an index named `product_info` is deleted.

**Note:** For information about other commands, see [open source Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html).

