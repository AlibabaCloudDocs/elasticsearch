# Preparations

This topic describes required preparations.

## Create a VPC and a VSwitch

For more information, see [Create an IPv4 VPC network](/intl.en-US/Quick Start/Create an IPv4 VPC network.md).

## Create Alibaba Cloud Elasticsearch clusters

Create two Alibaba Cloud Elasticsearch clusters to respectively act as the input and output of Alibaba Cloud Logstash. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md). This tutorial uses Elasticsearch V6.7 clusters of the Standard Edition and a Logstash V6.7 cluster as an example. The following figure shows the configurations of the Elasticsearch clusters.

![Cluster configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6506186061/p85389.png)

**Note:**

-   The source Elasticsearch cluster must be connected to the same VPC as your Logstash cluster. Otherwise, you must configure network and security settings to enable access to the Logstash cluster over the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet]().
-   The destination Elasticsearch cluster must reside in the same region, zone, and VPC as the Logstash cluster. In addition, the version of the destination Elasticsearch cluster must be the same as that of the Logstash cluster.
-   The scripts provided in this tutorial only apply to scenarios where a Logstash V6.7 cluster is used to synchronize data between Elasticsearch V6.7 clusters.
-   In this tutorial, the elastic account is used to access the Elasticsearch clusters. If you want to use a custom account, you must first create a role for the account and grant the required permissions to the role.

## Enable the Auto Indexing feature for Elasticsearch clusters

To ensure data security, Alibaba Cloud Elasticsearch disables Auto Indexing by default. Instead of calling the Create index operation, Logstash submits data to Elasticsearch. Elasticsearch then automatically indexes the data. Therefore, before you use Logstash to upload data, you must enable the Auto Indexing feature for Elasticsearch clusters. For more information about how to enable this feature, see [Enable the Auto Indexing feature](/intl.en-US/Quick Start/Step 2: (Optional) Configure a cluster.md).

## Prepare data

Log on to the Kibana console of the source Elasticsearch cluster. In the left-side navigation pane, click **Dev Tools**. On the **Console** tab of the page that appears, create an index and documents.

**Note:**

For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

The following code applies only to Elasticsearch V6.7 clusters.

-   Create an index named `my_index`

    ```
    PUT /my_index
    {
        "settings" : {
          "index" : {
            "number_of_shards" : "5",
            "number_of_replicas" : "1"
          }
        },
        "mappings" : {
            "my_type" : {
                "properties" : {
                  "post_date": {          
                       "type": "date"
                        "dynamic" : "true"       
                   },
                  "tags": {
                       "type": "keyword"
                   },
                    "title" : {
                        "type" : "text",
                        "analyzer" : "cjk"
                    }
                }
            }
        }
    }
    ```

-   Create a document named `1`

    ```
    PUT /my_index/my_type/1?pretty
    {
      "title": "One", 
      "tags": ["ruby"],
      "post_date":"2009-11-15T13:00:00"
    }
    ```

-   Create a document named `2`

    ```
    PUT /my_index/my_type/2?pretty
    {
      "title": "Two", 
      "tags": ["ruby"],
      "post_date":"2009-11-15T14:00:00"
    }
    ```


