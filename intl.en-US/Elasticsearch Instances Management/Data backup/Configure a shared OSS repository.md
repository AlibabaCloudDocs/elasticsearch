---
keyword: [shared OSS repository, restore data between Elasticsearch clusters]
---

# Configure a shared OSS repository

Alibaba Cloud Elasticsearch provides shared OSS repositories. You can restore data from the automatic snapshots of an Elasticsearch cluster that are stored in these repositories to a destination Elasticsearch cluster. For example, you have two Elasticsearch V6.7.0 clusters es-cn-a and es-cn-b. The Auto Snapshot feature is enabled for the es-cn-a cluster, and a snapshot is created for the cluster. If you want to restore the data in the snapshot to the es-cn-b cluster, specify a shared OSS repository in the es-cn-b cluster.

The source and destination Elasticsearch clusters must meet the following requirements:

-   The clusters reside in the same region.
-   The clusters belong to the same Alibaba Cloud account.
-   The version of the source cluster is earlier than or the same as that of the destination cluster.

    If the versions of the source and destination clusters are both V6.7.0 of the Standard Edition, the latest kernel must be used for the clusters. Alternatively, the kernel version of the destination cluster must be later than that of the source cluster.

    **Note:**

    -   An Elasticsearch cluster can use only the repository for an Elasticsearch cluster of the same or an earlier version.
    -   When a cluster uses the repository for a cluster of an earlier version, the cluster may be incompatible with the data format of the cluster of an earlier version. For example, you can restore an index that has only one document type to an Elasticsearch V6.7.0 cluster from snapshots in the repository of an Elasticsearch V5.5.3 cluster. However, if you restore an index that has multiple document types to the Elasticsearch V6.7.0 cluster from snapshots in the repository of the Elasticsearch V5.5.3 cluster, an error may occur. This is because Elasticsearch V6.7.0 clusters do not support indexes that have multiple document types.

## Add a shared OSS repository

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Snapshots**.

5.  In the **Shared OSS Repositories** section, click **Create Now**.

    **Note:** If it is not the first time you add a shared OSS repository, click **Create Shared Repository**.

6.  In the **Create Shared Repository** dialog box, select an Elasticsearch cluster.

    **Note:** The selected cluster must meet the preceding requirements.

7.  Click **OK**.

    After the shared repository is added, the current page shows the cluster that owns the repository and the repository status.

    ![Shared repository successfully added](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4767819951/p63593.png)

    **Note:** The system uses your Elasticsearch cluster to retrieve the repository list. If the cluster is updating its configuration, is abnormal, or encounters heavy workloads, the system may fail to retrieve the repository list. If this error occurs, you can log on to the Kibana console and run the `GET _snapshot` command to retrieve the endpoints of all repositories. For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

8.  Restore an index.

    **Shared OSS repositories** are only used to share data between Elasticsearch clusters. The system cannot directly restore data for clusters from the shared OSS repositories. If you want to restore an index to an Elasticsearch cluster, run the related command in the Kibana console of the cluster. For example, to restore the file-2019-08-25 index from the es-cn-a cluster, perform the following steps:

    1.  Log on to the Kibana console of the Elasticsearch cluster to which you want to restore the index.

        For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

    2.  In the left-side navigation pane, click **Dev Tools**.

    3.  On the **Console** tab of the page that appears, run the following command to query the information of all snapshots in the repository of the es-cn-a cluster:

        ```
        GET /_cat/snapshots/aliyun_snapshot_from_es-cn-a?v
        ```

        The command returns information about all the snapshots in the repository.

        ![Information about all the snapshots in the repository](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4767819951/p63598.png)

        **Note:** `aliyun_snapshot_from_es-cn-a` is the name of the **shared repository** that is added in [Add a shared OSS repository](#section_zf4_nr6_ie2).

    4.  Restore indexes from the snapshot based on the returned information.

        **Note:**

        -   Before you restore an index, make sure that the destination cluster does not have an index with the same name. If the destination cluster has an index with the same name, make sure that the index is disabled. If the index is enabled, an error occurs during index restoration.
        -   Indexes whose names start with `.` are system indexes. We recommend that you do not restore these indexes. If you restore these indexes, you may fail to access the Kibana console.
        -   Restore a single index

            ```
            POST _snapshot/aliyun_snapshot_from_es-cn-a/es-cn-a_20190705220000/_restore 
             {"indices": "file-2019-08-25"}
            ```

        -   Restore multiple indexes

            ```
            POST _snapshot/aliyun_snapshot_from_es-cn-a/es-cn-a_20190705220000/_restore
             {"indices": "kibana_sample_data_ecommerce,kibana_sample_data_logs"}
            ```

        -   Restore all indexes other than indexes whose names start with `.`

            ```
            POST _snapshot/aliyun_snapshot_from_es-cn-a/es-cn-a_20190705220000/_restore 
            {"indices":"*,-.monitoring*,-.security*,-.kibana*","ignore_unavailable":"true"}
            ```


