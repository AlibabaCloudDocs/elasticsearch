# Preparations

This topic describes the required preparations.

## Create a VPC and a vSwitch

For more information, see [Create an IPv4 VPC network](/intl.en-US/Quick Start/Create an IPv4 VPC network.md).

## Create Alibaba Cloud Elasticsearch clusters

Create two Alibaba Cloud Elasticsearch clusters to respectively act as the input and output of Alibaba Cloud Logstash. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md). The following figure shows the configurations of the clusters.

![configurations of the clusters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0705960161/p127592.png)

In this topic, a Logstash V6.7.0 cluster is used to migrate data between Elasticsearch V6.7.0 clusters of the Standard Edition. The code provided in this topic applies only to this type of data migration.

**Note:**

-   The Logstash cluster and source Elasticsearch cluster must reside in the same virtual private cloud \(VPC\). If they reside in different VPCs, you must configure network and security settings to enable access to the Logstash cluster over the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet](/intl.en-US/Logstash/Network and security/Configure a NAT gateway for data transmission over the Internet.md).
-   The destination Elasticsearch cluster must reside in the same region, zone, and VPC as the Logstash cluster. In addition, the versions of the destination Elasticsearch cluster and Logstash cluster must meet [compatibility requirements](/intl.en-US/Product Introduction/Compatibility matrixes.md).
-   The default username that is used to access the Alibaba Cloud Elasticsearch clusters is elastic. If you want to use a user other than elastic, you must assign the required roles and permissions to the user.

## Enable the Auto Indexing feature for the destination Elasticsearch cluster

To ensure data security, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. When you use Logstash to migrate data between Elasticsearch clusters, indexes are created by submitting data instead of calling the create index operation. Therefore, before you use Logstash to migrate data, you must enable the **Auto Indexing** feature for the destination Elasticsearch cluster. For more information about how to enable this feature, see [Enable the Auto Indexing feature](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 2: (Optional) Configure a cluster.md).

## Prepare data

Log on to the Kibana console of the source Elasticsearch cluster. In the left-side navigation pane, click **Dev Tools**. On the **Console** tab of the page that appears, create an index and documents.

**Note:**

-   For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).
-   The following code applies only to Elasticsearch V6.7.0 clusters.

1.  Create an index named `my_index`.

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
                   },
                  "tags": {
                       "type": "keyword"
                   },
                    "title" : {
                        "type" : "text"
                    }
                }
            }
        }
    }
    ```

2.  Create a document named `1`.

    ```
    PUT /my_index/my_type/1?pretty
    {
      "title": "One", 
      "tags": ["ruby"],
      "post_date":"2009-11-15T13:00:00"
    }
    ```

3.  Create a document named `2`.

    ```
    PUT /my_index/my_type/2?pretty
    {
      "title": "Two", 
      "tags": ["ruby"],
      "post_date":"2009-11-15T14:00:00"
    }
    ```


