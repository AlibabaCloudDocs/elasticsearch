---
keyword: Elasticsearch synonym
---

# Upload a synonym dictionary file

Before you use synonyms, you must upload a synonym dictionary file. This topic describes how to upload a synonym dictionary file to your Alibaba Cloud Elasticsearch cluster and provides the related precautions.

## Precautions

-   Before you upload a synonym dictionary file to your Elasticsearch cluster, you must make sure that the cluster is in a normal state. After you upload a synonym dictionary file, the system restarts the cluster. During the restart, the system updates the synonym dictionaries on all the nodes in the cluster based on the synonym dictionary file. The time required for the updated dictionaries to take effect depends on the specifications, data volume, and load of the cluster. We recommend that you upload a synonym dictionary file during off-peak hours.
-   In most cases, if the indexes of your cluster have replica shards and the load of your cluster is normal, your cluster can still provide services during a cluster modification. The following items indicate that the load of a cluster is normal: The CPU utilization of the cluster is about 60%, the heap memory usage of the cluster is about 50%, and the value of NodeLoad\_1m is less than the number of vCPUs for the cluster.

-   If the indexes of your cluster do not have replica shards, the load of the cluster is excessively high, and large amounts of data are written to or queried in your cluster, access timeouts may occur during a cluster modification. We recommend that you configure an access retry mechanism for your client before you perform a cluster modification. This reduces the impact on your business.

-   A new dictionary file does not take effect for existing indexes because these indexes cannot automatically load the file. For example, the `index-aliyun` index is created based on the aliyun.txt synonym file, and you modify the file and upload the modified file. The uploaded file does not take effect for the index. If you also want a new dictionary file to take effect on existing indexes, reindex the data in these indexes after the synonym dictionary of your cluster is updated.
-   A synonym dictionary file must be a TXT file encoded in UTF-8. Each line can contain only one synonym expression. Synonym expressions support [Solr rules](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-synonym-tokenfilter.html#_solr_synonyms) and [WordNet rules](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-synonym-tokenfilter.html#_wordnet_synonyms). The following code provides an example:

    ```
    ipod, i-pod, i pod => ipod, i-pod, i pod
    foo => foo bar
    ```

-   The stopword list of your Elasticsearch cluster cannot contain the keywords specified in the synonym dictionary file of the cluster. Otherwise, an error is reported in the logs of the cluster when you upload a new synonym dictionary file or make other changes.

## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  In the **Basic Configuration** section, click **Upload** next to **Synonym Dictionary Configuration**.

6.  In the **Synonym Dictionary Configuration** panel, select a method that you want to use to upload a synonym dictionary file. Then, upload the file based on the following instructions.

    **Note:** A synonym dictionary file is a TXT file generated based on the rules in [Configuration rules](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure synonyms/Configuration rules.md).

    ![Configure synonyms](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6467819951/p60220.png)

    -   **Upload**: If you select this method, click **Upload** and select the synonym dictionary file that you want to upload from your on-premises machine.
    -   **Add OSS File**: If you select this method, specify Bucket Name and File Name, and click **Add**.

        Make sure that the bucket resides in the same region as your Elasticsearch cluster and the file that you want to upload is a TXT file.

7.  Click **Save**.


After the state of the Elasticsearch cluster becomes Active, log on to the Kibana console of the cluster. Then, create indexes, verify synonyms, and upload test data to perform a search test. When you create an index, you must specify the settings and mapping parameters and specify the `"synonyms_path": "analysis/your_dict_name.txt"` setting for the settings parameter. For more information, see [Configure synonyms](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure synonyms/Configure synonyms.md) and [Using Synonyms](https://www.elastic.co/guide/en/elasticsearch/guide/2.x/using-synonyms.html) in open source Elasticsearch documentation.

