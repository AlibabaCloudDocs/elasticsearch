# Use ILM to separate hot data from cold data

This topic describes how to use the index lifecycle management \(ILM\) feature to separate hot data from cold data in an Alibaba Cloud Elasticsearch cluster. The separation enables you to implement the hot-warm architecture. This architecture improves the read/write performance of the cluster, automates the maintenance of hot and cold data, and reduces your production costs.

In the era of big data, data constantly changes. Data stored in Elasticsearch increases over time. When the data volume reaches a specific level, the memory usage, CPU utilization, and I/O throughput also increase. This affects the full-text search capability of Elasticsearch. To address this issue, Elasticsearch V6.6.0 and later provide the [ILM](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/index-lifecycle-management.html) feature. You can use this feature to create, set, enable, disable, or delete Elasticsearch indexes throughout their lifecycle. You can use the feature for time series data, cold data, and hot data to reduce data storage costs. This topic uses cold and hot data to demonstrate how to use the ILM feature. Business scenario:

1.  Write data to the indexes of an Elasticsearch cluster in real time. When the data volume in the cluster reaches a specific level, the system automatically rolls over data to new indexes.
2.  The new indexes stay in the hot phase for 30 minutes and enter the warm phase.
3.  In the warm phase, the system shrinks the new indexes and merges the segments in the indexes. The indexes stay in the warm phase for 30 minutes and enter the cold phase.
4.  In the cold phase, data is migrated from hot nodes to warm nodes to separate hot data from cold data. The indexes are deleted one hour later.

## Recommended configurations

-   You must configure ILM policies based on your business model. For example, we recommend that you configure different aliases and ILM policies for indexes with different structures. This facilitates index management.
-   The name of an initial index must end with an auto-increment six-digit number, such as -000001. Otherwise, ILM policies cannot take effect. For example, an initial index is named myindex-000001. After a rollover, a new index named myindex-000002 is generated. If the names of your indexes do not meet the preceding requirements, we recommend that you reindex your data.
-   In the hot phase, the system writes data. To ensure that data is written in chronological order, we recommend that you do not write data to indexes in the warm or cold phase. For example, for the warm phase, set `actions` to `shrink` or `read only`. This way, indexes are read only after they enter the warm phase.

    **Note:** For more information about each lifecycle phase, see [Use ILM to manage Heartbeat indexes](/intl.en-US/Best Practices/Elasticsearch applications/Index management/Use ILM to manage Heartbeat indexes.md).

-   You configure more vCPUs and use disks with higher I/O performance for hot nodes to process hot data. You configure more disk space for warm nodes to store cold data. Warm nodes can still provide services even if you configure fewer vCPUs and use disks with lower I/O performance for them.

## Configure an ILM policy for indexes

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to view the attributes of nodes:

    ```
    GET _cat/nodeattrs?v&h=host,attr,value
    ```

    If the command is successfully executed, the result shown in the following figure is returned. This figure shows that the Elasticsearch cluster contains three hot nodes and three warm nodes to support the hot-warm architecture.

    ![Hot-warm architecture](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0635742061/p139891.png)

    **Note:** When you purchase an Elasticsearch cluster, you must purchase warm nodes. The system then automatically deploys the hot-warm architecture. For more information, see [t1914387.md\#]().

4.  Call an API operation to define an ILM policy.

    ```
    PUT /_ilm/policy/game-policy
    {
      "policy": {
        "phases": {
          "hot": {
            "actions": {
              "rollover": {
                "max_size": "1GB",
                "max_age": "1d",
                "max_docs": 1000
              }
            }
          },
          "warm": {
            "min_age": "30m",
            "actions": {
              "forcemerge": {
                    "max_num_segments":1
                  },
              "shrink": {
                    "number_of_shards":1
                  }
            }
          },
          "cold": {
            "min_age": "1h",
            "actions": {
              "allocate": {
                "require": {
                  "box_type": "warm"
                }
              }
            }
          },
          "delete": {
            "min_age": "2h",
            "actions": {
              "delete": {}
            }
          }
        }
      }
    }
    ```

    **Note:**

    -   After an ILM policy is created, you cannot change the policy name.
    -   In this step, you can specify the `max_age` parameter in the minimum unit of seconds. If you use the Kibana console to create an ILM policy, you can specify this parameter only in the minimum unit of hours.
5.  Create an index template.

    In the `settings` configuration, specify the hot attribute. This way, data can be stored in hot nodes after it is written.

    ```
    PUT _template/gamestabes_template
    {
      "index_patterns" : ["gamestabes-*"],
      "settings": {
        "index.number_of_shards": 5,
        "index.number_of_replicas": 1,
        "index.routing.allocation.require.box_type":"hot",
        "index.lifecycle.name": "game-policy", 
        "index.lifecycle.rollover_alias": "gamestabes"
      }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |`index.routing.allocation.require.box_type`|The type of nodes to which newly created indexes are allocated.|
    |`index.lifecycle.name`|The name of the ILM policy.|
    |`index.lifecycle.rollover_alias`|The alias of the index that is generated during a rollover.|

6.  Create an index based on an auto-increment number.

    ```
    PUT gamestabes-000001
    {
    "aliases": {
        "gamestabes":{
           "is_write_index": true
            }
          }
    }
    ```

    You can also create an index based on time. For more information, see [Using date math](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-rollover-index.html#_using_date_math_with_the_rollover_api).

7.  Use the index alias to write data.

    The system periodically checks for indexes that match an ILM policy. If the system finds matched indexes, it rolls over the data in the indexes.

    ```
    PUT gamestabes/_doc/1
    {
        "EU_Sales" : 3.58,
        "Genre" : "Platform",
        "Global_Sales" : 40.24,
        "JP_Sales" : 6.81,
        "Name" : "Super Mario Bros.",
        "Other_Sales" : 0.77,
        "Platform" : "NES",
        "Publisher" : "Nintendo",
        "Year_of_Release" : "1985",
        "na_Sales" : 29.08
    }
    ```

    **Note:** By default, the system checks for indexes that match an ILM policy at 10-minute intervals. You can specify the `indices.lifecycle.poll_interval` parameter to change the [check interval](https://www.elastic.co/guide/en/elasticsearch/reference/7.0/ilm-settings.html). After the data in an index is rolled over, it enters the next phase.

8.  Filter indexes based on lifecycle phases and view detailed index configurations.

    1.  In the left-side navigation pane, click **Management**.

    2.  In the **Elasticsearch** section, click **Index Management**.

    3.  In the **Index management** section, click **Lifecycle phase** next to **Lifecycle status** and select a phase.

        ![Filter indexes based on lifecycle phases](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0635742061/p140000.png)

    4.  Click an index name to view its details.

        ![View index details](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0635742061/p140002.png)


## Verify data distribution

1.  Query indexes in the cold phase and view their configurations.

    ![Query indexes in the cold phase](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0635742061/p140004.png)

2.  Query the distribution of shards for indexes in the cold phase.

    ```
    GET _cat/shards?shrink-gamestables-000012
    ```

    If the command is successfully executed, the result shown in the following figure is returned. This figure shows that data in the indexes is mainly distributed on warm nodes.

    ![Command output](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0635742061/p140007.png)


## Update the ILM policy

1.  Update the running ILM policy.

    ![Update the ILM policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0635742061/p140013.png)

2.  View the version of the updated policy.

    1.  In the left-side navigation pane, click **Management**.

    2.  In the **Elasticsearch** section, click **Index Lifecycle Policies**.

    3.  In the **Index lifecycle policies** section, view the version of the updated policy.

        The version number of the updated policy is one more than that of the original policy. The updated policy takes effect from the next rollover.

        ![View the version of the updated policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0635742061/p140015.png)


## Switch the ILM policy

1.  Create another ILM policy.

    ```
    PUT /_ilm/policy/game-new
    {
      "policy": {
        "phases": {
          "hot": {
            "actions": {
              "rollover": {
                "max_size": "3GB",
                "max_age": "1d",
                "max_docs": 1000
              }
            }
          },
          "warm": {
            "min_age": "30m",
            "actions": {
              "forcemerge": {
                    "max_num_segments":1
                  },
              "shrink": {
                    "number_of_shards":1
                  }
            }
          },
          "cold": {
            "min_age": "1h",
            "actions": {
              "allocate": {
                "require": {
                  "box_type": "warm"
                }
              }
            }
          },
          "delete": {
            "min_age": "2h",
            "actions": {
              "delete": {}
            }
          }
        }
      }
    }
    ```

2.  Associate the new policy with the index template.

    ```
    PUT _template/gamestabes_template
    {
      "index_patterns" : ["gamestabes-*"],
      "settings": {
        "index.number_of_shards": 5,
        "index.number_of_replicas": 1,
        "index.routing.allocation.require.box_type":"hot",
        "index.lifecycle.name": "game-new", 
        "index.lifecycle.rollover_alias": "gamestabes"
      }
    }
    ```

    **Note:**

    -   The new policy takes effect from the next rollover.
    -   If you want to associate the new policy with the indexes that are created based on the original policy, you can run the `PUT gamestabes-*/_settings` command. For more information, see [Switching policies for an index](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/_switching_policies_for_an_index.html).

## Summary

This topic provides instructions on how to separate hot data from cold data by using ILM.

-   [Configure an ILM policy for indexes](#section_v57_pmk_c5q).

    Procedure:

    1.  Configure hot and warm attributes.
    2.  Configure an index template based on your needs.
    3.  Configure an ILM policy based on your needs and associate the policy with the index template.
    4.  Create an initial index whose name ends with -000001. The name of the index generated after a rollover is automatically incremented by one.
-   [Verify data distribution](#section_p83_r9w_z2s).

    Check whether the shards of indexes in the cold phase are distributed on warm nodes.

-   [Update the ILM policy](#section_dti_qh0_jy2).

    Update the ILM policy.

-   [Switch the ILM policy](#section_bju_m4i_g71).

    Switch the ILM policy.


