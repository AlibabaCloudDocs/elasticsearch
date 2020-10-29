# Connect two Elasticsearch clusters

To ensure data security, Alibaba Cloud Elasticsearch clusters are isolated from each other by default. If you want to search for data across two Elasticsearch clusters, you can connect the clusters. This topic describes how to connect two Elasticsearch clusters.

Make sure that the two Elasticsearch clusters meet the following requirements:

-   They are of the same Elasticsearch version.
-   They belong to the same Alibaba Cloud account.
-   They reside in the same virtual private cloud \(VPC\).
-   They use the same deployment method.

## Configure the connection between two Elasticsearch clusters

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the Basic Information page, click **Security**.

5.  On the page that appears, click **Edit** next to **Cluster Interconnection**.

6.  In the **Edit Configuration** pane, click **Add Cluster**.

7.  In the **Add Cluster** dialog box, select the ID of the remote Elasticsearch cluster that you want to connect.

    ![Add Cluster dialog box](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0767819951/p61517.png)

    If one or more Elasticsearch clusters meet the prerequisites, you can select these clusters in the **Add Cluster** dialog box.

    **Note:**

    -   The RAM users of an Alibaba Cloud account can query all Elasticsearch clusters that belong to the account only after the ListInstance permission is granted to the RAM users. For more information, see [Types of resources that can be authorized](/intl.en-US/RAM/Types of resources that can be authorized.md).
    -   After you connect the current Elasticsearch cluster to a remote cluster, you can view the ID of the current cluster on the **Edit Configuration** pane of the remote cluster. This indicates that the communication between the two clusters is bidirectional.
8.  Click **OK**.

    After you add a cluster, you can find the added cluster in the **Connected Clusters** section of the **Edit Configuration** pane.

    ![Connected Clusters section](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0767819951/p61518.png)

    **Note:** If you no longer require the added cluster, click **Remove** in the Actions column to remove the cluster.


After you complete the configuration, you must perform the operations described in [Configure the cross-cluster search feature](#section_cge_km0_eay). This way, you can search for data in the remote Elasticsearch cluster from the current Elasticsearch cluster.

## Configure the cross-cluster search feature

1.  Log on to the Kibana console of the remote Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  Create an index, add a document to the index, and insert data into the document in the remote Elasticsearch cluster.

    -   Create an index

        ```
        PUT /twitter
        {
            "settings" : {
                "index" : {
                    "number_of_shards" : 3, 
                    "number_of_replicas" : 2 
                }
            }
        }
        ```

    -   Create a document and insert data into the document

        ```
        POST twitter/doc/
        {
            "user" : "kimchy",
            "post_date" : "2009-11-15T14:12:12",
            "message" : "trying out Elasticsearch"
        }
        ```

    **Note:** The index and document are used to test the cross-cluster search feature.

3.  Log on to the Kibana console of the current Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

4.  Use one of the following methods to configure the cross-cluster search feature in the current Elasticsearch cluster.

    In the following methods, an Elasticsearch V6.7 cluster is used. The methods that are used to configure the cross-cluster search feature in other Elasticsearch versions are similar. For more information, see [Cross-cluster search in open source Elasticsearch 7.X](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/modules-cross-cluster-search.html), [Cross-cluster search in open source Elasticsearch 6.3](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-cross-cluster-search.html), and [Cross-cluster search in open source Elasticsearch 5.5](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/modules-cross-cluster-search.html).

    -   Method 1: Use the internal endpoint of the remote Elasticsearch cluster

        ```
        PUT _cluster/settings
        {
          "persistent": {
            "cluster": {
              "remote": {
                "cluster_one": {
                  "seeds": [
                    "es-cn-o4xxxxxxxxxxxx4f1.elasticsearch.aliyuncs.com:9300"
                  ]
                }
              }
            }
          }
        }
        ```

    -   Method 2: Use the IP addresses of the nodes in the remote Elasticsearch cluster

        ```
        PUT _cluster/settings
        {
          "persistent": {
            "cluster": {
              "remote": {
                "cluster_one": {
                  "seeds": [
                    "10.8.xx.xx:9300",
                    "10.8.xx.xx:9300",
                    "10.8.xx.xx:9300"
                  ]
                }
              }
            }
          }
        }
        ```

    **Note:**

    -   If the Elasticsearch cluster is deployed in a single zone, you can use Method 1 or Method 2. If the Elasticsearch cluster is deployed across zones, you can use only Method 2. Both methods can be used to connect multiple remote Elasticsearch clusters to the current Elasticsearch cluster.
    -   You can query the index data in the remote Elasticsearch cluster from the current Elasticsearch cluster. This only applies if you configured the domain name or node IP addresses of the remote Elasticsearch cluster in the current Elasticsearch cluster for cross-cluster searches. You cannot run similar commands in the remote Elasticsearch cluster to search for data in the current Elasticsearch cluster. To search for data in the current Elasticsearch cluster from the remote Elasticsearch cluster, you must add the domain name or node IP addresses of the current cluster in the remote cluster.
5.  Run the following command to check whether the cross-cluster search feature is correctly configured:

    ```
    POST /cluster_one:twitter/doc/_search
    {
      "query": {
        "match_all": {}
      }
    }
    ```

    If the cross-cluster search feature is correctly configured, the following result is returned:

    ```
    {
      "took" : 78,
      "timed_out" : false,
      "_shards" : {
        "total" : 3,
        "successful" : 3,
        "skipped" : 0,
        "failed" : 0
      },
      "_clusters" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0
      },
      "hits" : {
        "total" : 1,
        "max_score" : 1.0,
        "hits" : [
          {
            "_index" : "cluster_one:twitter",
            "_type" : "doc",
            "_id" : "qudxxxxxxxxxx_7ie6J",
            "_score" : 1.0,
            "_source" : {
              "user" : "kimchy",
              "post_date" : "2009-11-15T14:12:12",
              "message" : "trying out Elasticsearch"
            }
          }
        ]
      }
    }
    ```


