# Use method

The aliyun-sql plug-in is developed based on Apache Calcite and deployed on a server. It is used to parse SQL queries. After you install this plug-in on your Alibaba Cloud Elasticsearch cluster, you can execute SQL statements to query data in the cluster the same way as in common databases. This greatly reduces the training and usage costs of Elasticsearch.

You have completed the following operations:

-   An Alibaba Cloud Elasticsearch cluster is created. The cluster version is 6.7.0 or later.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

    **Note:** The aliyun-sql plug-in is available only for Alibaba Cloud Elasticsearch clusters of V6.7.0 or later.

-   The aliyun-sql plug-in is installed.

    By default, the aliyun-sql plug-in is installed on the Elasticsearch cluster. You can check whether the plug-in is installed on the plug-in configuration page. If the plug-in is not installed, follow the instructions provided in [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Install and remove a built-in plug-in.md) to install the plug-in.


The aliyun-sql plug-in offers more features than open source SQL plug-ins. The following table compares the aliyun-sql plug-in with open source SQL plug-ins.

|SQL plug-in|SQL parser|Paged query|Join|Nested|Common function|Case Function|Extended UDF|Optimization of execution plans|
|-----------|----------|-----------|----|------|---------------|-------------|------------|-------------------------------|
|x-pack-sql \(6.x\)|Antlr|Supported.|Not supported.|Supported. The syntax is a.b.|Supports abundant functions.|Not supported.|Not supported.|Provides a large number of rules for the optimization of execution plans.|
|opendistro-for-elasticsearch|Druid|Not supported. The maximum number of data entries that can be queried is determined by the `max_result_window` parameter.|Supported.|Supported. The syntax is `nested(message.info)`.|Supports a few functions.|Not supported.|Not supported.|Provides a few rules for the optimization of execution plans.|
|aliyun-sql|Javacc|Supported.|Supported. The plug-in provides the truncation feature. This feature allows you to dynamically configure the number of data entries that you can query from a single table. For more information, see [Syntax overview](#section_hle_s8a_v7z).|Supported. The syntax is a.b.|Supports abundant functions. For more information, see [Other functions and expressions](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the aliyun-sql plug-in/Query syntax.md).|Supported.|Supported. For more information, see [UDFs](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the aliyun-sql plug-in/Query syntax.md).|Provides a large number of rules for the optimization of execution plans and uses [Calcite](https://calcite.apache.org/docs/index.html) to optimize execution plans.|

**Note:** The CASE statement in the preceding table refers to the CASE WHEN THEN ELSE syntax.

## Precautions

-   Before you use the plug-in, make sure that the `aliyun.sql.enabled` parameter is set to true for your Elasticsearch cluster. You can set the parameter in the Kibana console. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).
-   You can manually remove the plug-in. Before you remove the plug-in, run the following command in the Kibana console to disable it. To disable the plug-in, set `aliyun.sql.enabled` to null. The following example shows the configuration:

    ```
    PUT _cluster/settings
    {
      "persistent": {
        "aliyun.sql.enabled": null
      }
    }
    ```

    Plug-in removal restarts your cluster. If you do not disable the plug-in before you remove it, your cluster remains stuck in the restart. In this case, you must run the following command to clear the archiving configurations and resume the restart:

    ```
    PUT _cluster/settings
    {
      "persistent": {
        "archived.aliyun.sql.enabled": null
      }
    }
    ```


## Syntax overview

The aliyun-sql plug-in uses the syntax of MySQL 5.0 and supports a wide range of functions and expressions. For more information, see [Other functions and expressions](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the aliyun-sql plug-in/Query syntax.md).

-   Basic queries

    ```
    SELECT [DISTINCT] (* | expression) [[AS] alias] [, ...]
    FROM table_name
    [WHERE condition]
    [GROUP BY expression [, ...]
     [HAVING condition]]
    [ORDER BY expression [ ASC | DESC ] [, ...]]
    [LIMIT [offset, ] size]
    ```

-   JOIN queries

    ```
    SELECT
      expression
    FROM table_name
    JOIN table_name 
     ON expression
    [WHERE condition] 
    ```

    **Note:**

    -   When you perform a JOIN query, Alibaba Cloud Elasticsearch limits the maximum number of data entries that you can query from a single table. The default number is 10,000. You can specify the maximum number of queries by setting `max.join.size`.
    -   The JOIN query you performed is an INNER JOIN query. Actually, the aliyun-sql plug-in performs a merge join for the query. When you perform a JOIN query, make sure that the field values in the tables you want to join change with the document IDs of Elasticsearch. JOIN queries can be performed only for fields of a numeric data type.

## Procedure

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  Enable the aliyun-sql plug-in.

    ```
    PUT _cluster/settings
    {
      "transient": {
        "aliyun.sql.enabled": true
      }
    }
    ```

3.  Initiate a write request.

    **Note:** The aliyun-sql plug-in supports only query requests. Therefore, the following code uses a bulk request to write data.

    -   Data of student information

        ```
        PUT stuinfo/_doc/_bulk? refresh
        {"index":{"_id":"1"}}
        {"id":572553,"name":"xiaoming","age":"22","addr":"addr1"}
        {"index":{"_id":"2"}}
        {"id":572554,"name":"xiaowang","age":"23","addr":"addr2"}
        {"index":{"_id":"3"}}
        {"id":572555,"name":"xiaoliu","age":"21","addr":"addr3"}
        ```

    -   Data of student rankings

        ```
        PUT sturank/_doc/_bulk? refresh
        {"index":{"_id":"1"}}
        {"id":572553,"score":"90","sorder":"5"}
        {"index":{"_id":"2"}}
        {"id":572554,"score":"92","sorder":"3"}
        {"index":{"_id":"3"}}
        {"id":572555,"score":"86","sorder":"10"}
        ```

4.  Execute an SQL statement.

    Perform a JOIN query to query the name and ranking of a student.

    ```
    POST /_alisql
    {
      "query":"select stuinfo.name,sturank.sorder from stuinfo join sturank on stuinfo.id=sturank.id"
    }
    ```

    If the statement is successfully executed, the aliyun-sql plug-in returns table information. The `columns` field contains column names and data types. The `rows` field contains row data.

    ```
    {
      "columns" : [
        {
          "name" : "name",
          "type" : "text"
        },
        {
          "name" : "sorder",
          "type" : "text"
        }
      ],
      "rows" : [
        [
          "xiaoming",
          "5"
        ],
        [
          "xiaowang",
          "3"
        ],
        [
          "xiaoliu",
          "10"
        ]
      ]
    }
    ```


