---
keyword: [create manual snapshots for Elasticsearch data, snapshot API, restore Elasticsearch data from manual snapshots]
---

# Create manual snapshots and restore data from manual snapshots

Alibaba Cloud Elasticsearch provides some commands for you to create manual snapshots for the index data stored on your Elasticsearch cluster, store the snapshots in a shared repository, or restore data from the snapshots. This topic describes how to create manual snapshots and restore data from the snapshots.

## Precautions

-   Before you use these commands to create manual snapshots for your index data or restore data from the snapshots, make sure that your cluster is in a normal state. Otherwise, the creation of the manual snapshots or the restoration of the data is affected.
-   Snapshots store only index data. The following information of an Elasticsearch cluster is not stored in snapshots: monitoring data \(such as indexes whose names start with `.monitoring` or `.security_audit`\), metadata, translogs, configurations, software packages, built-in and custom plug-ins, and logs.
-   You can run all the code provided in this topic in the Kibana console of your Elasticsearch cluster. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Prerequisites

Object Storage Service \(OSS\) is activated, and an OSS bucket is created.

**Note:** The storage class of the OSS bucket must be Standard. Elasticsearch does not support the Archive storage class. In addition, the OSS bucket must reside in the same region as your Elasticsearch cluster.

For more information, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md) and [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).

## Create a repository

Create a repository named my\_backup.

**Note:** When you create a repository, use the PUT request method. When you update the settings of the newly created repository, use the POST request method rather than the PUT request method. If you use the PUT request method for the update, the settings of existing repositories are also updated.

```
PUT _snapshot/my_backup/
{
    "type": "oss",
    "settings": {
        "endpoint": "http://oss-cn-hangzhou-internal.aliyuncs.com",
        "access_key_id": "xxxx",
        "secret_access_key": "xxxxxx",
        "bucket": "xxxxxx",
        "compress": true,
        "chunk_size": "500mb",
        "base_path": "snapshot/"
    }
}
```

|Parameter|Description|
|---------|-----------|
|endpoint|The internal endpoint of the OSS bucket. For more information about how to obtain the endpoint, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
|access\_key\_id|The AccessKey ID of your account. For more information about how to obtain the AccessKey ID, see [Obtain an AccessKey pair]().|
|secret\_access\_key|The AccessKey secret of your account. For more information about how to obtain the AccessKey secret, see [Obtain an AccessKey pair]().|
|bucket|The name of the OSS bucket. For more information about how to obtain the name, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).|
|compress|Specifies whether the data compression feature is enabled for snapshots. Valid values: -   true: indicates that the data compression feature is enabled for snapshots. This feature applies only to index metadata, including the mappings and settings of indexes.
-   false: indicates that the data compression feature is disabled for snapshots. The default value is false. |
|chunk\_size|If you want to upload large volumes of data to the OSS bucket, you can upload the data in multiple parts. In this case, you can use this parameter to set the size of each part. If the size of a part reaches the value of this parameter, the excess data is distributed to another part.|
|base\_path|The start location of the repository. The default value is the root directory.|

## Query repository information

-   Query the information of all repositories

    ```
    GET _snapshot
    ```

-   Query the information of a specific repository

    ```
    GET _snapshot/my_backup
    ```


## Create a snapshot

-   Create a snapshot for all enabled indexes

    ```
    PUT _snapshot/my_backup/snapshot_1
    ```

    The preceding command creates the snapshot\_1 snapshot for all the enabled indexes and stores the snapshot in the my\_backup repository. After you run the command, the system immediately returns a response and creates the snapshot. If you want the system to return a response after the snapshot is created, specify the wait\_for\_completion parameter in the command. This parameter blocks all API calls until the snapshot is created. If the total size of the indexes is large, the response is returned after a long period of time.

    ```
    PUT _snapshot/my_backup/snapshot_1?wait_for_completion=true
    ```

    **Note:**

    -   A repository stores multiple snapshots. Each snapshot is a copy of all indexes, specific indexes, or a single index in a cluster.
    -   The first snapshot is a full copy of the data in a cluster. Subsequent snapshots store only incremental data. If you create a subsequent snapshot, the system only adds data to or removes data from the previous snapshot. Therefore, less time is required to create a subsequent snapshot than the first snapshot.
-   Create a snapshot for specific indexes

    By default, a snapshot contains all the enabled indexes. If Kibana is used when you create a snapshot, you may want to ignore all diagnostic indexes \(the `.kibana` indexes\) because of limited disk space. In this case, you can run the following command to create a snapshot only for specific indexes:

    ```
    PUT _snapshot/my_backup/snapshot_2
    {
        "indices": "index_1,index_2"
    }
    ```

    The preceding command creates a snapshot only for the index1 and index2 indexes.


## Query snapshot information

-   Query the information of all snapshots

    ```
    GET _snapshot/my_backup/_all
    ```

    If the command is successfully run, the following result is returned:

    ```
    {
      "snapshots": [
        {
          "snapshot": "snapshot_1",
          "uuid": "vIdSCkthTeGa0nSj4D****",
          "version_id": 5050399,
          "version": "5.5.3",
          "indices": [
            ".kibana"
          ],
          "state": "SUCCESS",
          "start_time": "2018-06-28T01:22:39.609Z",
          "start_time_in_millis": 1530148959609,
          "end_time": "2018-06-28T01:22:39.923Z",
          "end_time_in_millis": 1530148959923,
          "duration_in_millis": 314,
          "failures": [],
          "shards": {
            "total": 1,
            "failed": 0,
            "successful": 1
          }
        },
        {
          "snapshot": "snapshot_3",
          "uuid": "XKO_Uwz_Qu6mZrU3Am****",
          "version_id": 5050399,
          "version": "5.5.3",
          "indices": [
            ".kibana"
          ],
          "state": "SUCCESS",
          "start_time": "2018-06-28T01:25:00.764Z",
          "start_time_in_millis": 1530149100764,
          "end_time": "2018-06-28T01:25:01.482Z",
          "end_time_in_millis": 1530149101482,
          "duration_in_millis": 718,
          "failures": [],
          "shards": {
            "total": 1,
            "failed": 0,
            "successful": 1
          }
        }
      ]
    }
    ```

-   Query the information of a specific snapshot based on the snapshot name

    ```
    GET _snapshot/my_backup/snapshot_3
    ```

    If the command is successfully run, the following result is returned:

    ```
    {
      "snapshots": [
        {
          "snapshot": "snapshot_3",
          "uuid": "vIdSCkthTeGa0nSj4D****",
          "version_id": 5050399,
          "version": "5.5.3",
          "indices": [
            ".kibana"
          ],
          "state": "SUCCESS",
          "start_time": "2018-06-28T01:22:39.609Z",
          "start_time_in_millis": 1530148959609,
          "end_time": "2018-06-28T01:22:39.923Z",
          "end_time_in_millis": 1530148959923,
          "duration_in_millis": 314,
          "failures": [],
          "shards": {
            "total": 1,
            "failed": 0,
            "successful": 1
          }
        }
      ]
    }
    ```

-   Call the \_status API to query the information of a specific snapshot

    ```
    GET _snapshot/my_backup/snapshot_3/_status
    ```

    The \_status API allows you to query the detailed information of a snapshot. The information includes both the status of the snapshot and the statistics about each index and shard. If the command is successfully run, the following result is returned:

    ```
    {
    "snapshots": [
       {
          "snapshot": "snapshot_3",
          "repository": "my_backup",
          "state": "IN_PROGRESS", 
          "shards_stats": {
             "initializing": 0,
             "started": 1, 
             "finalizing": 0,
             "done": 4,
             "failed": 0,
             "total": 5
          },
          "stats": {
             "number_of_files": 5,
             "processed_files": 5,
             "total_size_in_bytes": 1792,
             "processed_size_in_bytes": 1792,
             "start_time_in_millis": 1409663054859,
             "time_in_millis": 64
          },
          "indices": {
             "index_3": {
                "shards_stats": {
                   "initializing": 0,
                   "started": 0,
                   "finalizing": 0,
                   "done": 5,
                   "failed": 0,
                   "total": 5
                },
                "stats": {
                   "number_of_files": 5,
                   "processed_files": 5,
                   "total_size_in_bytes": 1792,
                   "processed_size_in_bytes": 1792,
                   "start_time_in_millis": 1409663054859,
                   "time_in_millis": 64
                },
                "shards": {
                   "0": {
                      "stage": "DONE",
                      "stats": {
                         "number_of_files": 1,
                         "processed_files": 1,
                         "total_size_in_bytes": 514,
                         "processed_size_in_bytes": 514,
                         "start_time_in_millis": 1409663054862,
                         "time_in_millis": 22
                    }
                 }
              }
            }
          }
        }
      ]
    }
    ```


## Delete a snapshot

You can run the following command to delete a specific snapshot. If the snapshot is being created, the system stops the creation and deletes the snapshot from the repository.

```
DELETE _snapshot/my_backup/snapshot_3
```

**Note:**

-   You can delete snapshots only by calling the DELETE API. A snapshot may be associated with the data in other snapshots. If some data in the snapshot that you want to delete is associated with other snapshots, the DELETE API finds the data and deletes only the data that is not associated with other snapshots.
-   If you choose to manually delete a snapshot, you may delete the data that is being used by other snapshots. This can cause data loss.

## Restore indexes from a snapshot

**Note:** We recommend that you do not restore system indexes whose names start with a period \(`.`\). If you restore these indexes, you may fail to access the Kibana console.

-   Restore all the indexes in a specific snapshot to your Elasticsearch cluster

    ```
    POST _snapshot/my_backup/snapshot_1/_restore
    ```

    -   For example, if the snapshot\_1 snapshot contains five indexes, the preceding command restores all these indexes to the Elasticsearch cluster.
    -   After you call the \_restore API, the system immediately returns a response and restores the indexes. If you want to block all API calls until the restoration is complete, you can specify the wait\_for\_completion parameter in the command.

        ```
        POST _snapshot/my_backup/snapshot_1/_restore?wait_for_completion=true
        ```

-   Restore all the indexes in a specific snapshot, excluding system indexes whose names start with a period \(`.`\)

    ```
    POST _snapshot/my_backup/snapshot_1/_restore 
    {"indices":"*,-.monitoring*,-.security*,-.kibana*","ignore_unavailable":"true"}
    ```

-   Restore a specific index in a snapshot to your Elasticsearch cluster and rename the index

    If you only want to verify or process the data in the index and do not want to overwrite the data in the cluster, use this method to restore the index.

    ```
    POST /_snapshot/my_backup/snapshot_1/_restore
    {
     "indices": "index_1", 
     "rename_pattern": "index_(.+)", 
     "rename_replacement": "restored_index_$1" 
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |indices|The name of the index that you want to restore. In this example, the system restores only the index\_1 index from the specified snapshot.|
    |rename\_pattern|The format of the index name. The system searches for the index that you want to restore based on its name. The name must match the format you specified.|
    |rename\_replacement|The regular expression that is used to rename the index.|


## Query restoration information

You can call the \_recovery API to query the information of an index restoration task, such as the status and progress of the task.

-   Query the restoration information of a specific index

    ```
    GET restored_index_3/_recovery
    ```

-   Query the restoration information of all indexes \(The information may include the information of shards that are not involved in the restoration process.\)

    ```
    GET /_recovery/
    ```

    The following result is returned:

    ```
    {
       "restored_index_3" : {
         "shards" : [ {
           "id" : 0,
           "type" : "snapshot",
           "stage" : "index",
           "primary" : true,
           "start_time" : "2014-02-24T12:15:59.716",
           "stop_time" : 0,
           "total_time_in_millis" : 175576,
           "source" : {
             "repository" : "my_backup",
             "snapshot" : "snapshot_3",
             "index" : "restored_index_3"
           },
           "target" : {
             "id" : "ryqJ5lO5S4-lSFbGnt****",
             "hostname" : "my.fqdn",
             "ip" : "10.0.**.**",
             "name" : "my_es_node"
           },
           "index" : {
             "files" : {
               "total" : 73,
               "reused" : 0,
               "recovered" : 69,
               "percent" : "94.5%"
             },
             "bytes" : {
               "total" : 79063092,
               "reused" : 0,
               "recovered" : 68891939,
               "percent" : "87.1%"
             },
             "total_time_in_millis" : 0
           },
           "translog" : {
             "recovered" : 0,
             "total_time_in_millis" : 0
           },
           "start" : {
             "check_index_time" : 0,
             "total_time_in_millis" : 0
           }
         } ]
       }
    }
    ```

    The returned result lists all the indexes that are being restored and all the shards of these indexes. The result contains the following information for each shard: restoration start time, restoration end time, restoration duration, restoration progress, and transmitted bytes. The following table describes some parameters in the preceding result.

    |Parameter|Description|
    |---------|-----------|
    |type|The type of the restoration. The value snapshot indicates that the shard is being restored from a snapshot.|
    |source|The snapshot and repository to which the shard belongs.|
    |percent|The progress of the restoration. The value 94.5% indicates that 94.5% of the data in the shard is restored.|


## Cancel index restoration

You can use the DELETE command to delete an index that is being restored to cancel the restoration of the index.

```
DELETE /restored_index_3
```

If the restored\_index\_3 index is being restored, the preceding command stops the restoration and deletes the data that is restored to the cluster.

## References

-   [Snapshot And Restore](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/modules-snapshots.html)
-   [elasticsearch-repository-oss](https://github.com/aliyun/elasticsearch-repository-oss)
-   [Create automatic snapshots and restore data from automatic snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create automatic snapshots and restore data from automatic snapshots.md)

