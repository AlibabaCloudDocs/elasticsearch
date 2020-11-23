---
keyword: [use OSS to migrate data in a self-managed Elasticsearch cluster, migrate Elasticsearch data]
---

# Use OSS to migrate data from a self-managed Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster

You can use the snapshots that are stored in Object Storage Service \(OSS\) to migrate data from a self-managed Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster. To migrate data, call the snapshot operation to create a snapshot for the self-managed Elasticsearch cluster. Then, store the snapshot in OSS and restore data from the snapshot to your Alibaba Cloud Elasticsearch cluster. This topic describes the detailed procedure.

OSS is used to migrate large volumes of data.

## Procedure

1.  [Preparations](#section_crk_fhz_uvr)

    Prepare a self-managed Elasticsearch cluster, and create an OSS bucket and an Alibaba Cloud Elasticsearch cluster.

2.  [Step 1: Install the elasticsearch-repository-oss plug-in](#section_nx5_kz4_1xs)

    Install the elasticsearch-repository-oss plug-in on each node of the self-managed Elasticsearch cluster. You can create an OSS repository for the self-managed Elasticsearch cluster only after the plug-in is installed.

3.  [Step 2: Create a snapshot repository for the self-managed Elasticsearch cluster](#section_ftm_9c8_55t)

    Call the snapshot operation to create a snapshot repository for the self-managed Elasticsearch cluster.

4.  [Step 3: Create a snapshot for specific indexes](#section_euo_abh_8w0)

    Create a snapshot for the indexes whose data you want to migrate and store the snapshot in the created snapshot repository.

5.  [Step 4: Create the same snapshot repository for the Alibaba Cloud Elasticsearch cluster](#section_rr1_qrd_wxk)

    In the Kibana console of your Alibaba Cloud Elasticsearch cluster, call the snapshot operation to create a snapshot repository for the cluster. The snapshot repository must have the same name as the snapshot repository for the self-managed Elasticsearch cluster.

6.  [Step 5: Restore data to the Alibaba Cloud Elasticsearch cluster from the created snapshot](#section_q2f_ayf_3kk)

    Restore data from the snapshot in the snapshot repository of the self-managed Elasticsearch cluster to your Alibaba Cloud Elasticsearch cluster.

7.  [Step 6: View restoration results](#section_9h6_0ur_tln)

    View the restored indexes and their data.


## Preparations

1.  Prepare a self-managed Elasticsearch cluster.

    We recommend that you deploy an Elasticsearch cluster on an Alibaba Cloud Elastic Compute Service \(ECS\) instance. For more information, see [Installing and Running Elasticsearch](https://www.elastic.co/guide/cn/elasticsearch/guide/current/running-elasticsearch.html).

    A single-node Elasticsearch V6.7.0 cluster is used in this topic. In actual production, you can purchase multiple ECS instances that reside in the same virtual private cloud \(VPC\) to deploy an Elasticsearch cluster. For more information about how to purchase an ECS instance, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

2.  Activate OSS, and create a bucket in the region where the ECS instance that hosts the self-managed Elasticsearch cluster resides.

    For more information, see [Activate OSS](/intl.en-US/Quick Start/Sign up for OSS.md) and [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

    **Note:** The storage class of the bucket must be Standard. Elasticsearch does not support the Archive storage class.

3.  Create an Alibaba Cloud Elasticsearch cluster in the region where the bucket you created resides.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).


## Step 1: Install the elasticsearch-repository-oss plug-in

1.  Connect to the ECS instance that hosts the self-managed Elasticsearch cluster.

    **Note:** For more information, see [Connect to a Linux instance](/intl.en-US/Instance/Connect to instances/Connect to Linux instances/Connect to a Linux instance by using VNC.md).

2.  Download the installation package of the elasticsearch-repository-oss plug-in.

    In this topic, the version of the plug-in is V6.7.0, which requires JDK 8.0 or later.

    ```
    wget https://github.com/aliyun/elasticsearch-repository-oss/releases/download/v6.7.0/elasticsearch-repository-oss-6.7.0.zip
    ```

3.  Decompress the installation package to the plugins folder in the installation path for the self-managed Elasticsearch cluster on the ECS instance.

    ```
    unzip -d /usr/local/elasticsearch-6.7.0/plugins/elasticsearch-repository-oss elasticsearch-repository-oss-6.7.0.zip
    ```

    You can also use a command to install the plug-in.

    ```
    ./bin/elasticsearch-plugin install file:///usr/local/elasticsearch-repository-oss-6.7.0.zip
    ```

4.  Start the ECS instance that hosts the self-managed Elasticsearch cluster.

    ```
    cd /usr/local/elasticsearch-6.7.0/bin
    ./elasticsearch -d
    ```


## Step 2: Create a snapshot repository for the self-managed Elasticsearch cluster

Connect to the ECS instance that hosts the self-managed Elasticsearch cluster and run the following command to create a snapshot repository:

```
curl -H "Content-Type: application/json" -XPUT localhost:9200/_snapshot/es_backup -d' {"type": "oss", "settings": { "endpoint": "http://oss-cn-hangzhou-internal.aliyuncs.com",  "access_key_id": "your_accesskeyid",  "secret_access_key":"your_accesskeysecret", "bucket": "es-backup-es", "compress": true }}'
```

|Parameter|Description|
|---------|-----------|
|`es_backup`|The name of the repository, which can be customized.|
|`type`|The type of the repository. Set the value to `oss`.|
|`endpoint`|The endpoint of your OSS bucket. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md). **Note:** If the ECS instance that hosts the self-managed Elasticsearch cluster resides in the same region as your OSS bucket, use the internal endpoint of the OSS bucket. Otherwise, use the public endpoint. |
|`access_key_id`|The AccessKey ID of the Alibaba Cloud account that is used to create the OSS bucket. For more information about how to obtain the AccessKey ID, see [How can I obtain an AccessKey ID and AccessKey secret?](/intl.en-US/FAQ/General FAQ/How can I obtain an AccessKey ID and AccessKey secret?.md)|
|`secret_access_key`|The AccessKey secret of the Alibaba Cloud account that is used to create the OSS bucket. For more information about how to obtain the AccessKey secret, see [How can I obtain an AccessKey ID and AccessKey secret?](/intl.en-US/FAQ/General FAQ/How can I obtain an AccessKey ID and AccessKey secret?.md)|
|`bucket`|The name of your OSS bucket.|
|`compress`|Specifies whether to enable compression.|

If the repository is created, `"acknowledge":true` is returned.

## Step 3: Create a snapshot for specific indexes

Create a snapshot for indexes whose data you want to migrate. By default, all enabled indexes are backed up in the snapshot. If you do not want to back up system indexes, such as indexes whose names start with `.kibana`, `.security`, or `.monitoring`, you can specify the indexes that you want to back up.

**Note:** We recommend that you do not back up system indexes because they occupy large storage space.

```
curl -H "Content-Type: application/json" -XPUT localhost:9200/_snapshot/es_backup/snapshot_1? pretty -d'
{
"indices": "index1,index2"
}'
```

`index1` and `index2` indicate the names of the indexes that you want to back up. If the snapshot is created, `"accepted" : true` is returned.

## Step 4: Create the same snapshot repository for the Alibaba Cloud Elasticsearch cluster

1.  Log on to the Kibana console of the Alibaba Cloud Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to create a snapshot repository that has the same name as the snapshot repository for the self-managed Elasticsearch cluster.

    ```
    PUT _snapshot/es_backup
    {
        "type": "oss",
        "settings": {
            "endpoint": "oss-cn-hangzhou-internal.aliyuncs.com",
            "access_key_id": "your_accesskeyid",
            "secret_access_key": "your_accesskeysecret",
            "bucket": "es-backup-es",
            "compress": true
        }
    }
    ```


## Step 5: Restore data to the Alibaba Cloud Elasticsearch cluster from the created snapshot

In the Kibana console of the Alibaba Cloud Elasticsearch cluster, run the following command to restore all indexes \(except system indexes whose names start with `.`\) from the created snapshot. Follow the instructions in the "[Step 4: Create the same snapshot repository for the Alibaba Cloud Elasticsearch cluster](#section_rr1_qrd_wxk)" section to perform the restoration.

```
POST _snapshot/es_backup/snapshot_1/_restore
{"indices":"*,-.monitoring*,-.security_audit*","ignore_unavailable":"true"}
```

If the command is successfully executed, `"accepted" : true` is returned.

The preceding command restores all indexes in the snapshot. You can also specify the indexes that you want to restore. In the Alibaba Cloud Elasticsearch cluster, an existing index may have the same name as an index you want to restore. In this case, if you do not want to overwrite the data in the existing index, you can rename the index you want to restore during the restoration.

```
POST _snapshot/es_backup/snapshot_1/_restore
{
  "indices":"index1",
  "rename_pattern": "index(.+)",
  "rename_replacement": "restored_index_$1"
}
```

**Note:** For more information about the commands that are used to create snapshots or restore data, see [Commands to create snapshots and restore data](/intl.en-US/Elasticsearch Instances Management/Data backup/Commands for creating snapshots and restoring data.md).

## Step 6: View restoration results

In the Kibana console of the Alibaba Cloud Elasticsearch cluster, run the following command to view the restoration results. Follow the instructions in the "[Step 4: Create the same snapshot repository for the Alibaba Cloud Elasticsearch cluster](#section_rr1_qrd_wxk)" section to perform the operation.

-   View the restored indexes

    ```
    GET /_cat/indices?v
    ```

    ![View the restored indexes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5330852061/p113311.png)

-   View the data in the restored indexes

    ```
    GET /index1/_search
    ```

    If the command is successfully executed, the following result is returned:

    ```
    {
      "took" : 2,
      "timed_out" : false,
      "_shards" : {
        "total" : 5,
        "successful" : 5,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : 1,
        "max_score" : 1.0,
        "hits" : [
          {
            "_index" : "index1",
            "_type" : "_doc",
            "_id" : "1",
            "_score" : 1.0,
            "_source" : {
              "productName" : "testpro",
              "annual_rate" : "3.22%",
              "describe" : "testpro"
            }
          }
        ]
      }
    }
    ```


