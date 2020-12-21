---
keyword: [standard update of IK dictionaries, rolling update of IK dictionaries, Elasticsearch stopword list]
---

# Use the analysis-ik plug-in

analysis-ik is an IK analysis plug-in of Alibaba Cloud Elasticsearch. This plug-in cannot be removed. In addition to open source features, the plug-in can dynamically load the dictionaries that are stored in Object Storage Service \(OSS\). The plug-in also allows you to use the standard or rolling update method to update dictionaries. This topic describes how to use the plug-in.

The analysis-ik plug-in supports two update methods for IK dictionaries: standard update and rolling update. For more information, see [Perform a standard update for IK dictionaries](#ik) and [Perform a rolling update for IK dictionaries](#section_5qm_xjg_ouf). The following table provides more details about the two methods.

|Update method|Application mode|Loading mode|Description|
|-------------|----------------|------------|-----------|
|Standard update|This method updates the dictionaries on all nodes in an Elasticsearch cluster. It requires a restart of the cluster for the update to take effect.|The system sends an uploaded dictionary file to all nodes in an Elasticsearch cluster, modifies the IKAnalyzer.cfg.xml file, and then restarts the nodes to load the file.|You can use the standard update method to update the built-in IK main dictionary and stopword list of the analysis-ik plug-in. In the Standard Update pane, you can view the built-in main dictionary `SYSTEM_MAIN.dic` and the built-in stopword list `SYSTEM_STOPWORD.dic`.|
|Rolling update|The first time you upload a dictionary file, the dictionaries on all nodes in an Elasticsearch cluster are updated. The cluster needs to be restarted for the update to take effect. If the dictionary file that you upload has the same name as the existing dictionary file, the cluster does not need to be restarted. The dictionaries are directly loaded while the cluster is running.|If the content of a dictionary file changes, you can use this method to update the dictionaries on all nodes in an Elasticsearch cluster. After you upload the latest dictionary file, the nodes automatically load the file. If the dictionary file list changes when you perform a rolling update, all nodes in the cluster need to reload dictionary configurations. For example, when you upload a new dictionary file or delete an existing dictionary file, the changes are synchronized to the IKAnalyzer.cfg.xml file.

|When you upload a dictionary file for the first time, the system modifies the IKAnalyzer.cfg.xml file. After the dictionaries are updated, the cluster must be restarted for the update to take effect.|

**Note:**

New dictionaries apply only to data that is inserted after a standard or rolling update. If you want to apply the new dictionaries to both the existing data and new data, you must reindex the existing data.

If you choose the standard update method, you can modify the built-in main dictionary or stopword list. However, you cannot delete the built-in main dictionary or stopword list. The following modification methods are available:

-   If you want to update the built-in main dictionary, upload a dictionary file named **SYSTEM\_MAIN.dic**. The new dictionary file automatically overwrites the existing file. For more information, see [IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik).
-   If you want to update the built-in stopword list, upload a file named **SYSTEM\_STOPWORD.dic**. The new file automatically overwrites the existing file. For more information, see [IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik) and [Configure a stopword list](#section_3gd_iqv_b8p).

## Prerequisites

Your Elasticsearch cluster is in a normal state. You can check the cluster status on the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page.

## Perform a standard update for IK dictionaries

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Plug-ins**.

5.  On the **Built-in Plug-ins** tab, find the analysis-ik plug-in and click **Standard Update** in the **Actions** column.

    ![Standard update](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5567819951/p40216.png)

6.  In the **Standard Update** pane, click **Configure** in the lower-right corner.

7.  Select a method to upload a dictionary file from the drop-down list that is below the **IK Main Dictionary** section. Then, upload the dictionary file based on the following instructions.

    ![IK main dictionary](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5567819951/p40219.png)

    You can select the **Upload DIC File** or **Add OSS File** method.

    -   **Upload DIC File**: If you select this method, click **Upload DIC File** and select the local file that you want to upload.
    -   **Add OSS File**: If you select this method, specify Bucket Name and File Name, and click **Add**.

        Make sure that the bucket resides in the same region as your Elasticsearch cluster and the file to upload is a DIC file. If the content of the dictionary that is stored in OSS changes, you must manually upload the dictionary file again.

    **Warning:** The following operation restarts your Elasticsearch cluster. Before you perform this operation, make sure that the restart does not affect your business.

8.  Scroll down to the lower part of the pane, select **This operation will restart the cluster. Continue?**, and click **Save**.

    If you choose the standard update method, the system restarts your cluster no matter whether you upload a new dictionary file, remove a dictionary file, or update dictionary content.

9.  After the cluster is restarted, log on to the Kibana console of the cluster and run the following command to check whether the new dictionary file takes effect.

    **Note:** For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

    ```
    GET _analyze
    {
    "analyzer": "ik_smart",
    "text": ["Tokens in the new dictionary file"]
    }
    ```


## Perform a rolling update for IK dictionaries

1.  On the **Built-in Plug-ins** tab, find the analysis-ik plug-in and click **Rolling Update** in the **Actions** column.

    ![Rolling update](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5567819951/p40222.png)

2.  In the **Rolling Update** pane, click **Configure** in the lower-right corner.

3.  Select a method to upload a dictionary file from the drop-down list that is below the **IK Main Dictionary** section. Then, upload the dictionary file based on the following instructions.

    ![Plug-in configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5567819951/p40223.png)

    **Note:** You cannot use the rolling update method to modify the built-in main dictionary. If you want to modify the built-in main dictionary, use the standard update method.

    You can select the **Upload DIC File** or **Add OSS File** method.

    -   **Upload DIC File**: If you select this method, click **Upload DIC File** and select the local file that you want to upload.
    -   **Add OSS File**: If you select this method, specify Bucket Name and File Name, and click **Add**.

        Make sure that the bucket resides in the same region as your Elasticsearch cluster and the file to upload is a DIC file. The `dic_0.dic` file is used in the following operations. If the content of the dictionary that is stored in OSS changes, you must manually upload the dictionary file again.

    **Warning:** The following operation restarts your Elasticsearch cluster. Before you perform this operation, make sure that the restart does not affect your business.

4.  Scroll down to the lower part of the pane, select **This operation will restart the instance. Continue?**, and click **Save**. If this is the first time that you upload a dictionary file, the system automatically restarts the cluster.

    After you click Save, the system performs a rolling update for the cluster. After the rolling update is completed, the new dictionary takes effect.

    If you want to add or remove tokens from dictionaries, perform the following steps to modify the `dic_0.dic` file:

5.  In the Rolling Update pane, delete the existing `dic_0.dic` file and upload a new dictionary file. The new dictionary file must have the same name.

    This operation changes the content of the existing dictionary file in the cluster and uploads a new file that has the same name. The system does not need to restart the cluster for the update to take effect.

6.  Click **Save**.

    The analysis-ik plug-in on the nodes of the Elasticsearch cluster automatically loads the dictionary file. The time that is required by each node to load the dictionary file varies. It requires about two minutes for all nodes to load the dictionary file. You can log on to the Kibana console of the Elasticsearch cluster and run the following command multiple times to verify the new dictionary file.

    **Note:** For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

    ```
    GET _analyze
    {
    "analyzer": "ik_smart",
    "text": ["Tokens in the new dictionary file"]
    }
    ```


## Configure a stopword list

Alibaba Cloud Elasticsearch provides a built-in stopword list. The list contains the following predefined tokens: a, an, and, are, as, at, be, but, by, for, if, in, into, is, it, no, not, of, on, or, such, that, the, their, then, there, these, they, this, to, was, will, with.

You can perform the following steps to remove tokens from the stopword list:

1.  Download the default [IK configuration file](https://github.com/medcl/elasticsearch-analysis-ik/releases) from the official website of open source Elasticsearch.

2.  Decompress the downloaded package and open the stopword.dic dictionary file in the config folder.

3.  Remove the tokens that you no longer require and save the dictionary file.

4.  Change the name of the stopword.dic dictionary file to SYSTEM\_STOPWORD.dic.

5.  Upload the SYSTEM\_STOPWORD.dic file to your Elasticsearch cluster. The file automatically overwrites the existing stopword list.

6.  After the cluster is restarted, the new stopword list takes effect.


