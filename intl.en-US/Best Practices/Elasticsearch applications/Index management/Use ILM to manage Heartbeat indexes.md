---
keyword: [Elasticsearch ILM, use Elasticsearch ILM to manage indexes]
---

# Use ILM to manage Heartbeat indexes

Time series data increase over time. You can use the index lifecycle management \(ILM\) feature to periodically roll over the data to new indexes. This ensures high query efficiency and reduces query costs. As indexes age and fewer queries are required, you can migrate the indexes to a less expensive disk and reduce the numbers of primary and replica shards. This topic describes how to use ILM to manage Heartbeat indexes.

The ILM feature allows you to create, set, enable, disable, or delete Elasticsearch indexes throughout their lifecycle. This feature is available in Elasticsearch V6.6.0 and later. It divides the lifecycle of an index into four phases: hot, warm, cold, and delete. In the hot phase, the feature rolls over data in existing indexes to new ones. In other phases, it processes the new indexes. The following table describes these phases.

|Phase|Description|
|-----|-----------|
|hot|Time series data is written in real time. You can call the [rollover operation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/indices-rollover-index.html) to roll over the data in existing indexes to new ones based on the number of documents, volume of data, and duration of these indexes.|
|warm|Data is no longer written to indexes. You can only query data from them.|
|cold|Indexes are no longer updated. Few queries are performed on these indexes, and the query process slows down.|
|delete|Data is deleted.|

Test scenario:

A large number of time series indexes whose names start with heartbeat- exist in your Elasticsearch cluster, and the size of a single index is about 4 MB each day. The number of shards increases with the data volume. This may cause cluster overload. In this case, you must configure different rollover policies for the four phases. In the hot phase, data in historical monitoring indexes whose names start with heartbeat- is rolled over to new indexes. In the warm phase, indexes are shrunk, and segments in each index are merged. In the cold phase, data is migrated from hot nodes to warm nodes. In the delete phase, data is deleted on a regular basis.

## Procedure

1.  [Preparations](#section_z64_ymo_p7z)

    Create an Alibaba Cloud Elasticsearch cluster. Then, enable the Auto Indexing feature for the cluster and configure a public IP address whitelist for the cluster.

2.  [Step 1: Enable and configure the ILM feature in the heartbeat.yml file](#section_i2s_5jq_een)

    In the heartbeat.yml file, enable and configure the ILM feature for the cluster. After the configuration is completed, the system automatically generates a Heartbeat index template for the cluster.

3.  [Step 2: Create an ILM policy](#section_q4f_09f_b6h)

    You can call the ILM policy operation to create an ILM policy. This policy defines the conditions to roll over data and archive indexes.

4.  [Step 3: Associate the ILM policy with an index template](#section_xie_dsu_xvj)

    Associate the ILM policy with the Heartbeat index template.

5.  [Step 4: Associate the ILM policy with an index](#section_73o_c19_inw)

    Associate the first index that is created by using the Heartbeat index template with the ILM policy. This way, the policy can apply to all indexes that are created by using this template.

6.  [Step 5: View indexes in different phases](#section_lew_8nm_29w)

    View the indexes that are archived in the hot, warm, cold, and delete phases.


## Preparations

1.  Create an Alibaba Cloud Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md) and [Enable auto indexing](/intl.en-US/Quick Start/Step 2 (optional): Configure a cluster.md).

2.  Configure a public IP address whitelist for the cluster. You must add the IP address of the server on which Heartbeat is installed to the public IP address whitelist of the cluster.

    For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).


## Step 1: Enable and configure the ILM feature in the heartbeat.yml file

To manage Heartbeat indexes by using the ILM feature of Elasticsearch, you can configure the feature in the heartbeat.yml file. For more information, see [Set up index lifecycle management](https://www.elastic.co/guide/en/beats/heartbeat/6.8/ilm.html).

1.  Download the [Heartbeat installation package](https://www.elastic.co/cn/downloads/past-releases#heartbeat) and decompress it.

2.  Specify the `heartbeat.monitors`, `setup.template.settings`, `setup.kibana`, and `output.elasticsearch` configurations in the heartbeat.yml file.

    Sample configurations:

    ```
    heartbeat.monitors:
    - type: icmp
      schedule: '*/5 * * * * * *'
      hosts: ["47.111.xx.xx"]
    
    setup.template.settings:
      index.number_of_shards: 3
      index.codec: best_compression
      index.routing.allocation.require.box_type: "hot"
    
    setup.kibana:
    
      # Kibana Host
      # Scheme and port can be left out and will be set to the default (http and 5601)
      # In case you specify and additional path, the scheme is required: http://localhost:5601/path
      # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
      host: "https://es-cn-4591jumei00xxxxxx.kibana.elasticsearch.aliyuncs.com:5601"
    
    output.elasticsearch:
      # Array of hosts to connect to.
      hosts: ["es-cn-4591jumei00xxxxxx.elasticsearch.aliyuncs.com:9200"]
      ilm.enabled: true
      setup.template.overwrite: true
      ilm.rollover_alias: "heartbeat"
      ilm.pattern: "{now/d}-000001"
    
      # Enabled ilm (beta) to use index lifecycle management instead daily indices.
      #ilm.enabled: false
    
      # Optional protocol and basic auth credentials.
      #protocol: "https"
      username: "elastic"
      password: "<your_password>"
    ```

    The following table describes some parameters in the preceding configurations. For more information about other parameters, see [open source Heartbeat configuration documentation](https://www.elastic.co/guide/en/beats/heartbeat/6.7/configuration-heartbeat-options.html?spm=a2c4g.11186623.2.28.3a623279cDgZt0).

    |Parameter|Description|
    |---------|-----------|
    |`index.number_of_shards`|The number of primary shards. Default value: 1.|
    |`index.routing.allocation.require.box_type`|Specifies whether to write data to hot nodes.|
    |`ilm.enabled`|Specifies whether to enable the ILM feature. If this parameter is set to true, the feature is enabled.|
    |`setup.template.overwrite`|Specifies whether to overwrite the original index template. If you have loaded an index template of a specific version to Elasticsearch, you must set this parameter to true to overwrite the original index template with the loaded template.|
    |`ilm.rollover_alias`|Specifies the alias of the index that is generated during a rollover. Default value: `heartbeat-\{beat.version\}`.|
    |`ilm.pattern`|The index pattern that is generated during a rollover. [date math](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/indices-rollover-index.html#_using_date_math_with_the_rollover_api) is supported. Default value: \{now/d\}-000001. If a rollover condition is met, the system increments the last digit in the index name by one to generate a new index name. For example, an index generated after the first rollover is named heartbeat-2020.04.29-000001. If another rollover condition is met, Elasticsearch creates a index named heartbeat-2020.04.29-000002. |

    **Note:** If you change the setting of `ilm.rollover_alias` or `ilm.pattern` after an index template is loaded, you must set `setup.template.overwrite` to `true` to overwrite the original index template with the loaded index template.

3.  Start the Heartbeat service.

    ```
    sudo ./heartbeat -e
    ```


## Step 2: Create an ILM policy

Elasticsearch allows you to use API calls or the Kibana console to create an ILM policy. This step describes how to call the ILM policy operation to create an ILM policy.

**Note:** Heartbeat allows you to run the `./heartbeat setup --ilm-policy` command to load the default policy and write it to Elasticsearch. You can run the `./heartbeat export ilm-policy` command to export the default policy to stdout. Then, modify the default policy to manually create an ILM policy.

[Log on to the Kibana console of your Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) and run the following command:

```
PUT /_ilm/policy/hearbeat-policy
{
  "policy": {
    "phases": {
      "hot": {
        "actions": {
          "rollover": {
            "max_size": "5mb",
            "max_age": "1d",
            "max_docs": 100
          }
        }
      },
      "warm": {
        "min_age": "60s",
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
        "min_age": "3m",
        "actions": {
          "allocate": {
            "require": {
              "box_type": "warm"
            }
          }
        }
      },
      "delete": {
        "min_age": "1h",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
```

|Parameter|Description|
|---------|-----------|
|hot|A rollover is triggered if an index associated with the ILM policy meets one of the following conditions: -   The size of the written data exceeds 5 MB.
-   The index has been used for more than one day.
-   The number of documents in the index exceeds 100.

During the rollover, the system creates an index and starts the ILM policy again. The original index enters the warm phase 60 seconds after the rollover. **Note:** If the value of max\_docs, max\_size, or max\_age is reached during a `rollover`, Elasticsearch archives the index. |
|warm|After the index enters the warm phase, the system shrinks it down to a new index that has only one primary shard and merges segments in the index into one segment. The index enters the cold phase three minutes after the rollover starts.|
|cold|The system migrates the index from a hot node to a warm node. The index enters the delete phase one hour later.|
|delete|The index is deleted one hour later.|

## Step 3: Associate the ILM policy with an index template

After you start Heartbeat, the system automatically creates a Heartbeat index template in your Elasticsearch cluster. You must associate the ILM policy created in [Step 2: Create an ILM policy](#section_q4f_09f_b6h) with this index template.

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Management**.

3.  In the **Elasticsearch** section, click **Index Lifecycle Policies**.

4.  In the **Index lifecycle policies** section, find the ILM policy you created, click **Actions**, and select **Add policy to index template**.

    ![Add policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4034742061/p102589.png)

5.  In the dialog box that appears, select heartbeat from the **Index template** drop-down list and specify **Alias for rollover index**.

    ![Add policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4034742061/p102591.png)

6.  Click **Add policy**.


## Step 4: Associate the ILM policy with an index

After you start Heartbeat, the system automatically creates Heartbeat indexes in your Elasticsearch cluster. You must associate the first index with the ILM policy that is associated with the index template you created. For more information, see [Step 3: Associate the ILM policy with an index template](#section_xie_dsu_xvj).

1.  In the **Elasticsearch** section of the **Management** page, click **Index Management**.

2.  In the **Index management** section, find your index and click its name.

3.  On the **Summary** tab of the pane that appears, click **Manage** and select **Remove lifecycle policy** to remove the default policy of Heartbeat.

    ![Remove the default policy of Heartbeat](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4034742061/p102620.png)

4.  In the dialog box that appears, click **Remove policy**.

5.  Click **Manage** again and select **Add lifecycle policy**.

6.  In the dialog box that appears, select the ILM policy you created \(heartbeat-policy\) from the **Lifecycle policy** drop-down list and set **Index rollover alias** to heartbeat. Then, click **Add policy**.

    ![Add policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4034742061/p102622.png)

    If the association is successful, the information shown in the following figure appears.

    ![Association succeeded](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4034742061/p102623.png)


## Step 5: View indexes in different phases

To view indexes in the hot phase, select **Hot** from the **Lifecycle phase** drop-down list in the **Index management** section.

![View indexes in the hot phase](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4034742061/p102625.png)

You can use this method to view indexes in other phases.

## Appendix: Set the interval to execute an ILM policy

The system periodically checks for indexes that match an ILM policy. The default interval is 10 minutes. Then, the system rolls over the data in matched indexes. For example, you set max\_docs to 100 when you [create an ILM policy](#section_q4f_09f_b6h). In this case, if the system finds that the number of documents in an index exceeds 100 during a check, it triggers a rollover for the index. You can change the value of the `indices.lifecycle.poll_interval` parameter to control the check interval. This ensures that data in indexes is rolled over in a timely manner.

**Note:** Set this parameter to an appropriate value. A small value may cause node overload. In this example, this parameter is set to `1m`.

```
PUT _cluster/settings
{
  "transient": {
    "indices.lifecycle.poll_interval":"1m"
  }
}
```

## Additional information

-   An ILM policy can be added for an index only after an index template and an alias are configured for the index.
-   You can use the methods described in the following table to add an ILM policy.

    |Method|Description|
    |------|-----------|
    |Add an ILM policy to an index template|The added ILM policy applies to all the indexes that are created by using the index template.|
    |Add an ILM policy to a single index|The added ILM policy applies only to the index.|

-   If you modify an ILM policy during a rollover, the new policy takes effect from the next rollover.

