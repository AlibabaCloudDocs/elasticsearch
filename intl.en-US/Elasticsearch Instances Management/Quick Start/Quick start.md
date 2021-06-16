# Quick start

This topic describes how to create an Alibaba Cloud Elasticsearch cluster and access the cluster. It also describes how to call Elasticsearch RESTful APIs, create an index and a document, insert data into the document, search for data, and delete an index.

-   An Alibaba Cloud account is created.

    To create an Alibaba Cloud account, visit the [account registration page](https://account.alibabacloud.com/register/intl_register.htm).

-   A virtual private cloud \(VPC\) and a vSwitch are created.

    For more information, see [Create an IPv4 VPC](/intl.en-US/Quick Start/Create an IPv4 VPC.md).

-   The specifications and storage capacity of your cluster are evaluated.

    For more information, see [Evaluate specifications and storage capacity]().


## Scenarios

A finance service enterprise uses an online platform to manage its wealth management products. The enterprise uses conventional databases to provide the search feature for customers. The wealth management products provided by the enterprise offer satisfactory returns, and the customer base of the enterprise grows rapidly. The expansion of its business system and the increase of customer information cause the inherent issues of conventional databases to become noticeable. These issues include slow search responses, low accuracy, and the low performance of data service devices. To resolve these issues and improve customer satisfaction, the enterprise purchases the Alibaba Cloud Elasticsearch service. This topic uses the preceding scenario to describe how to use Alibaba Cloud Elasticsearch to create a cluster and search for data.

For example, the enterprise provides the following wealth management products:

```
{
"products":[
{"productName":"Daily Wealth Management for Comprehensive Health","annual_rate":"3.2200%","describe":"180-day wealth management product. Minimum investment of CNY 20,000. Low-risk investment. Select whether to receive push messages for returns."}
{"productName":"Western Tongbao","annual_rate":"3.1100%","describe":"90-day wealth management product. Minimum investment of CNY 10,000. Daily push messages when returns are credited to your account."}
{"productName":"Anxiang Livestock Industry","annual_rate":"3.3500%","describe":"270-day wealth management product. Minimum investment of CNY 40,000. Daily push messages when returns are immediately credited to your account."}
{"productName":"Monthly 5G Device Purchase Profit","annual_rate":"3.1200%","describe":"90-day wealth management product. Minimum investment of CNY 12,000. Daily push messages when returns are credited to your account."}
{"productName":"New Energy Power Wealth Management","annual rate":"3.0100%","describe":"30-day wealth management product. Minimum investment of CNY 8,000. Daily push messages for returns."}
{"productName":"Microcredit Profit","annual_rate":"2.7500%","describe":"3-day popular wealth management product. No service fees. Minimum investment of CNY 500. Push messages for returns."}
]
}
```

## Step 1: Create a cluster

1.  Go to the [Elasticsearch buy page](https://common-buy-intl.alibabacloud.com/?commodityCode=elasticsearch_intl#/buy).

2.  On the buy page, configure cluster launch settings.

    In this topic, a V6.7 cluster is used. For more information about the parameters on the buy page, see [Parameters on the buy page](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md).

3.  Click **Buy Now**.

4.  After the cluster is created, click **Console** to go to the **Overview** page of the Elasticsearch console.

5.  In the left-side navigation pane, click **Elasticsearch Clusters**.

6.  In the top navigation bar, select a resource group and a region. Then, on the **Elasticsearch Clusters** page, view the newly created cluster.


## Step 2: Access the cluster

After the state of the cluster changes from Initializing to Active, you can perform the following steps to use Kibana to access the cluster:

**Note:** You can also use a curl command or a client to access the cluster. For more information, see [Access an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).

1.  On the **Elasticsearch Clusters** page, find the newly created cluster and click its ID in the Cluster ID/Name column.

2.  In the left-side navigation pane of the page that appears, click **Data Visualization**.

3.  In the **Kibana** section, click **Access over the Internet**.

4.  On the Kibana logon page, enter the username and password and click Log in.

    The username is **elastic**. The password is the one that is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).

5.  On the page that appears, click **Explore on my own**.

    ![Logon success page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7702907161/p206794.png)

6.  In the left-side navigation pane, click **Dev Tools**. On the page that appears, click **Get to work**.

7.  On the **Console** tab, run the following command to access the cluster:

    ```
    GET /
    ```

    If the access is successful, the following result is returned:

    ```
    {
      "name" : "tgeAvZe",
      "cluster_name" : "es-cn-nif1z64qj003g****",
      "cluster_uuid" : "IZmmd9IGTmKzqiZiyz****",
      "version" : {
        "number" : "6.7.0",
        "build_flavor" : "default",
        "build_type" : "tar",
        "build_hash" : "c9c0c3a",
        "build_date" : "2020-12-01T08:00:27.556078Z",
        "build_snapshot" : false,
        "lucene_version" : "7.7.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
      },
      "tagline" : "You Know, for Search"
    }
                            
    ```


## Step 3: Create an index

Run one of the following commands to create an index named product\_info:

-   Command for Elasticsearch clusters earlier than V7.0

    ```
    PUT /product_info
    {
      "settings": {
        "number_of_shards": 5,
        "number_of_replicas": 1
      },
      "mappings": {
        "products": {
          "properties": {
            "productName": {
           "type": "text",
           "analyzer": "ik_smart"
          },
            "annual_rate":{
           "type":"keyword"
          },
            "describe": {
          "type": "text",
          "analyzer": "ik_smart"
        }
          }
        }
      }
    }
    ```

-   Command for Elasticsearch clusters of V7.0 or later

    ```
    PUT /product_info
    {
      "settings": {
        "number_of_shards": 5,
        "number_of_replicas": 1
      },
      "mappings": {
          "properties": {
            "productName": {
              "type": "text",
              "analyzer": "ik_smart"
            },
            "annual_rate":{
              "type":"keyword"
            },
            "describe": {
              "type": "text",
              "analyzer": "ik_smart"
            }
        }
      }
    }
    ```

    **Note:** Mapping types are deprecated in open source Elasticsearch 7.0 and later. However, these mapping types are still supported in versions earlier than Elasticsearch 7.0. If mapping types are used in Elasticsearch 7.0 and later, the system displays the error message `"type": "mapper_parsing_exception"`. For more information, see [Removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/removal-of-types.html#_what_are_mapping_types).


In the preceding example, an index named product\_info is created. In a version earlier than Elasticsearch V7.0, the index is of the products type. In Elasticsearch V7.0 or later, the index is of the \_doc type. The index contains the productName, annual\_rate, and describe fields.

If the running of the command is successful, the following result is returned:

```
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "product_info"
}
```

## Step 4: Create a document and insert data into it

Use the \_bulk API to insert multiple data records at a time.

**Note:** The following command is suitable only for Elasticsearch clusters earlier than V7.0. For Elasticsearch clusters of V7.0 or later, you must change products to \_doc.

```
POST /product_info/products/_bulk
{"index":{}}
{"productName":"Daily Wealth Management for Comprehensive Health","annual_rate":"3.2200%","describe":"180-day wealth management product. Minimum investment of CNY 20,000. Low-risk investment. Select whether to receive push messages for returns."}
{"index":{}}
{"productName":"Western Tongbao","annual_rate":"3.1100%","describe":"90-day wealth management product. Minimum investment of CNY 10,000. Daily push messages when returns are credited to your account."}
{"index":{}}
{"productName":"Anxiang Livestock Industry","annual_rate":"3.3500%","describe":"270-day wealth management product. Minimum investment of CNY 40,000. Daily push messages when returns are immediately credited to your account."}
{"index":{}}
{"productName":"Monthly 5G Device Purchase Profit","annual_rate":"3.1200%","describe":"90-day wealth management product. Minimum investment of CNY 12,000. Daily push messages when returns are credited to your account."}
{"index":{}}
{"productName":"New Energy Power Wealth Management","annual rate":"3.0100%","describe":"30-day wealth management product. Minimum investment of CNY 8,000. Daily push messages for returns."}
{"index":{}}
{"productName":"Microcredit Profit","annual_rate":"2.7500%","describe":"3-day popular wealth management product. No service fees. Minimum investment of CNY 500. Push messages for returns."}
```

If `"errors" : false` is returned, data is inserted into the created document.

## Step 5: Search for data

**Note:** The following command is suitable only for Elasticsearch clusters earlier than V7.0. For Elasticsearch clusters of V7.0 or later, you must change products to \_doc.

-   Full-text search

    Run the following command to search for products whose descriptions contain `Daily push messages when returns are credited to your account`:

    ```
    GET /product_info/products/_search
    {
      "query": {
        "match": {
          "describe": "Daily push messages when returns are credited to your account"
        }
      }
    }
    ```

    If the running of the command is successful, the following result is returned:

    ```
    {
      "took" : 21,
      "timed_out" : false,
      "_shards" : {
        "total" : 5,
        "successful" : 5,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : 6,
        "max_score" : 1.3968885,
        "hits" : [
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WLvWYXAB8Rql5AUxLqUU",
            "_score" : 1.3968885,
            "_source" : {
              "productName" : "Western Tongbao",
              "annual_rate" : "3.1100%",
              "describe" : "90-day wealth management product. Minimum investment of CNY 10,000. Daily push messages when returns are credited to your account."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WrvWYXAB8Rql5AUxLqUU",
            "_score" : 1.3968885,
            "_source" : {
              "productName" : "Monthly 5G Device Purchase Profit",
              "annual_rate" : "3.1200%",
              "describe" : "90-day wealth management product. Minimum investment of CNY 12,000. Daily push messages when returns are credited to your account."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WbvWYXAB8Rql5AUxLqUU",
            "_score" : 1.3547361,
            "_source" : {
              "productName" : "Anxiang Livestock Industry",
              "annual_rate" : "3.3500%",
              "describe" : "270-day wealth management product. Minimum investment of CNY 40,000. Daily push messages when returns are immediately credited to your account."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "W7vWYXAB8Rql5AUxLqUU",
            "_score" : 1.1507283,
            "_source" : {
              "productName" : "New Energy Power Wealth Management",
              "annual rate" : "3.0100%",
              "describe" : "30-day wealth management product. Minimum investment of CNY 8,000. Daily push messages for returns."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "XLvWYXAB8Rql5AUxLqUU",
            "_score" : 0.5753642,
            "_source" : {
              "productName" : "Microcredit Profit",
              "annual_rate" : "2.7500%",
              "describe" : "3-day popular wealth management product. No service fees. Minimum investment of CNY 500. Push messages for returns."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "V7vWYXAB8Rql5AUxLqUU",
            "_score" : 0.31854028,
            "_source" : {
              "productName" : "Daily Wealth Management for Comprehensive Health",
              "annual_rate" : "3.2200%",
              "describe" : "180-day wealth management product. Minimum investment of CNY 20,000. Low-risk investment. Select whether to receive push messages for returns."
            }
          }
        ]
      }
    }
    ```

    **Note:** Alibaba Cloud Elasticsearch allows you to use a tokenizer to search for data. It also allows you to sort the records in search results by score. In the preceding result, the descriptions of the first two products contain `Daily push messages when returns are credited to your account`, while the descriptions of the last two products contain only `push messages`. The higher the ranking of a product in the search result, the higher the matching degree and score of the product.

-   Search by condition

    Run the following command to search for products with an annualized rate of 3.0000% to 3.1300%:

    ```
    GET /product_info/products/_search
    {
      "query": {
        "range": {
          "annual_rate": {
            "gte": "3.0000%",
            "lte": "3.1300%"
          }
        }
      }
    }
    ```

    If the running of the command is successful, the following result is returned:

    ```
    {
      "took" : 10,
      "timed_out" : false,
      "_shards" : {
        "total" : 5,
        "successful" : 5,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : 2,
        "max_score" : 1.0,
        "hits" : [
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WLvWYXAB8Rql5AUxLqUU",
            "_score" : 1.0,
            "_source" : {
              "productName" : "Western Tongbao",
              "annual_rate" : "3.1100%",
              "describe" : "90-day wealth management product. Minimum investment of CNY 10,000. Daily push messages when returns are credited to your account."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WrvWYXAB8Rql5AUxLqUU",
            "_score" : 1.0,
            "_source" : {
              "productName" : "Monthly 5G Device Purchase Profit",
              "annual_rate" : "3.1200%",
              "describe" : "90-day wealth management product. Minimum investment of CNY 12,000. Daily push messages when returns are credited to your account."
            }
          }
        ]
      }
    }
    ```

    **Note:** The system finds products that meet your requirements based on the search conditions and displays the products in descending order.

    For more information about search methods, see [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/query-dsl.html).


## Step 6: \(Optional\) Delete the index

After you are familiar with Alibaba Cloud Elasticsearch, you can delete the index by running the following command to avoid wasting resources:

**Warning:** Proceed with caution because the deleted indexes cannot be recovered.

```
DELETE /product_info
```

If the index is deleted, the following result is returned:

```
{
  "acknowledged" : true
}
```

## Step 7: \(Optional\) Release a cluster

If you no longer require a cluster, you can release the cluster. After the cluster is released, you are no longer charged for the cluster. In addition, the data stored on the cluster is deleted and cannot be recovered. You can release only pay-as-you-go clusters.

**Warning:** After a cluster is released, data stored on the cluster cannot be recovered. We recommend that you back up data before you release a cluster. For more information, see [Data backup overview](/intl.en-US/Elasticsearch Instances Management/Data backup/View the snapshot feature.md).

1.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find your cluster and choose **More** \> **Release** in the **Actions** column.

    ![Release a cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9367819951/p97626.png)

2.  In the message that appears, click **OK**.


## What to do next

-   Learn how to access and configure an Alibaba Cloud Elasticsearch cluster. For more information, see [Access and configure an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).
-   Learn how to synchronize data from ApsaraDB RDS for MySQL to Alibaba Cloud Elasticsearch. For more information, see [Select a synchronization method](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Select a synchronization method.md).
-   Learn frequently asked questions about Alibaba Cloud Elasticsearch. For more information, see [FAQ](/intl.en-US/.md).

