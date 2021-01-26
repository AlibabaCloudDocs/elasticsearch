---
keyword: [Elasticsearch data backup, snapshot operation, Elasticsearch data restoration]
---

# Commands to create snapshots and restore data

You can call the snapshot operation to back up or restore data for your Alibaba Cloud Elasticsearch cluster. The snapshot operation retrieves the status and data of your cluster and stores them to a shared repository.

## Precautions

-   Snapshots store only index data. The following information of your Elasticsearch cluster is not stored in snapshots: monitoring data \(such as indexes whose names start with .monitoring or .security\_audit\), metadata, translogs, configurations, software packages, built-in and custom plug-ins, and logs.
-   This topic uses the following markers to provide descriptions for code: <1\>, <2\>, and <3\>. Remove these markers before you run the code.
-   You can run the code provided in this topic in the Kibana console of your Elasticsearch cluster. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Prerequisites

Object Storage Service \(OSS\) is activated and an OSS bucket is created.

**Note:** The storage class of the OSS bucket must be Standard. Elasticsearch does not support the Archive storage class. The OSS bucket must reside in the same region as your Elasticsearch cluster.

For more information, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md) and [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).

## Create a repository

```
PUT _snapshot/my_backup
{
   "type": "oss",
    "settings": {
        "endpoint": "http://oss-cn-hangzhou-internal.aliyuncs.com", <1>
        "access_key_id": "xxxx",
        "secret_access_key": "xxxxxx",
        "bucket": "xxxxxx", <2>
        "compress": true,
        "base_path": "snapshot/" <3>
    }
}
```

**Note:** For more information about how to obtain the values of `access_key_id` and `secret_access_key`, see [How can I obtain an AccessKey ID and AccessKey secret?](/intl.en-US/FAQ/General FAQ/How can I obtain an AccessKey ID and AccessKey secret?.md).

-   <1\>: The `endpoint` parameter specifies the internal endpoint of the OSS bucket. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
-   <2\>: The bucket parameter specifies the name of the OSS bucket. For more information about how to obtain the bucket name, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).
-   <3\>: The base\_path parameter specifies the path of the repository. The default value is the root directory.

## Set the size of each part

You can upload large volumes of data to an OSS bucket in multiple parts. When you upload the data, you can use the `chunk_size` parameter to set the size of each part.

```
POST _snapshot/my_backup/ <1>
{
    "type": "oss",
    "settings": {
        "endpoint": "http://oss-cn-hangzhou-internal.aliyuncs.com",
        "access_key_id": "xxxx",
        "secret_access_key": "xxxxxx",
        "bucket": "xxxxxx",
        "chunk_size": "500mb",
        "base_path": "snapshot/" <2>
    }
}
```

-   <1\>: Use the POST method rather than the PUT method. The POST method updates repository settings.
-   <2\>: The base\_path parameter specifies the path of the repository. The default value is the root directory.

## Query repository information

```
GET _snapshot
```

You can also use the `GET _snapshot/my_backup` command to query the information of a specified repository.

## Create a snapshot for all enabled indexes

The following command is a basic command that is used to create a snapshot:

```
PUT _snapshot/my_backup/snapshot_1
```

This command creates the `snapshot_1` snapshot for all enabled indexes. The snapshot is stored in the `my_backup` repository. After you run the command, the system immediately returns a response and creates the snapshot at the backend.

If you want the system to return a response after the snapshot is created, specify the `wait_for_completion` parameter in the command. Example:

```
PUT _snapshot/my_backup/snapshot_1?wait_for_completion=true
```

After you run the command, the system does not return a response until the snapshot is created. If the size of an index is large, the response is returned after a longer period of time.

**Note:** The first snapshot is a full copy of the data in a cluster. Subsequent snapshots store only incremental data. When you create a subsequent snapshot, the system only adds data to or removes data from the previous snapshot. This means that it requires less time to create a subsequent snapshot than the first snapshot.

## Create a snapshot for specified indexes

By default, a snapshot contains all enabled indexes. If Kibana is used when you create a snapshot, you may want to ignore all diagnostic indexes \(the `.kibana` indexes\) because of limited disk space. To create a snapshot for specified indexes, run the following command.

**Note:** A repository stores multiple snapshots. Each snapshot is a copy of all indexes, specified indexes, or a single index in a cluster. When you create a snapshot, make sure that the snapshot name is unique.

```
PUT _snapshot/my_backup/snapshot_2
{
    "indices": "index_1,index_2"
}
```

The preceding command creates a snapshot only for the `index1` and `index2` indexes.

## Query snapshot information

In some cases, you may need to query snapshot information. For example, a snapshot name that contains a date is hard to remember, such as `backup_2014_10_28`.

To query the information of a snapshot, send a `GET` request that contains both the repository name and snapshot name. Example:

```
GET _snapshot/my_backup/snapshot_2
```

The response contains the details of the snapshot:

```
{
"snapshots": [
   {
      "snapshot": "snapshot_2",
      "indices": [
         ".marvel_2014_28_10",
         "index1",
         "index2"
      ],
      "state": "SUCCESS",
      "start_time": "2014-09-02T13:01:43.115Z",
      "start_time_in_millis": 1409662903115,
      "end_time": "2014-09-02T13:01:43.439Z",
      "end_time_in_millis": 1409662903439,
      "duration_in_millis": 324,
      "failures": [],
      "shards": {
         "total": 10,
         "failed": 0,
         "successful": 10
      }
   }
]
}
```

You can replace the snapshot name in the preceding command with `_all` to query all snapshots in the repository. Example:

```
GET _snapshot/my_backup/_all
```

## Monitor snapshot creation progress

The `wait_for_completion` parameter provides a simple method for you to monitor the progress of a snapshot creation task. However, this parameter is not suitable for snapshot creation tasks of medium-size Elasticsearch clusters. You can use one of the following methods to query the details of a snapshot:

-   Send a `GET` request with the snapshot name specified. Example:

    ```
    GET _snapshot/my_backup/snapshot_3
    ```

    If the system is creating the snapshot when you run the preceding command, the information of the creation task is returned, such as the start time and duration of the task.

    **Note:** The preceding command shares a thread pool with the command used to create a snapshot. Therefore, if you create a snapshot for large shards, the preceding command has to wait until the resources that are used by the snapshot creation command in the thread pool are released.

-   Call the status operation to query the snapshot status.

    ```
    {
    "snapshots": [
       {
          "snapshot": "snapshot_3",
          "repository": "my_backup",
          "state": "IN_PROGRESS", <1>
          "shards_stats": {
             "initializing": 0,
             "started": 1, <2>
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
                   },
                   ...
    ```

    -   <1\>: the status of the snapshot. If a snapshot is being created, the value of the field is `IN_PROGRESS`.
    -   <2\>: the number of shards that are being transmitted. If the value 1 is returned, a shard is being transmitted to the snapshot, and the other four shards have been transmitted.

        The value of the `shards_stats` parameter contains the status of the snapshot and the statistics about each index and shard. This parameter allows you to obtain the details of the snapshot creation progress. A shard can be in one of the following states:

        -   `INITIALIZING`: The shard is verifying the status of the cluster to check whether the shard can be stored in a snapshot. In most cases, this process is fast.
        -   `STARTED`: Data is being transmitted to the repository.
        -   `FINALIZING`: Data is transmitted, and the shard is sending snapshot metadata.
        -   `DONE`: A snapshot is created for the shard.
        -   `FAILED`: An error occurred during the snapshot creation. The shard, index, or snapshot cannot be processed. You can view logs for more information.

## Use a snapshot to migrate data

To use a snapshot to migrate data from an Elasticsearch cluster to another, perform the following steps:

1.  Back up a snapshot to OSS.
2.  Create a snapshot repository on the destination cluster. The repository must use the OSS bucket that stores the snapshot.
3.  Set the `base_path` parameter to the path of the snapshot.
4.  Run the data restoration command on the destination cluster.

## Cancel a snapshot

To cancel a snapshot, run the following command when the snapshot is being created:

```
DELETE _snapshot/my_backup/snapshot_3
```

This command stops the snapshot creation process and deletes the snapshot that is being created from the repository.

## Restore indexes from a snapshot

**Note:** If you restore indexes whose names start with `.` from snapshots, you may fail to connect to Kibana. Such indexes are system indexes. We recommend that you do not restore these indexes from snapshots.

To restore indexes from a snapshot, run the command that is used in the "[Create a repository](#section_kfz_3my_zgb)" section on the Elasticsearch cluster to which you want to restore the indexes. You can use one of the following methods to restore indexes from a snapshot:

-   To restore indexes from a specified snapshot, append the `_restore` parameter to the snapshot name in the command to run. Example:

    ```
    POST _snapshot/my_backup/snapshot_1/_restore
    ```

    After you run this command, the system restores all indexes in the snapshot. For example, if the `snapshot_1` snapshot contains five indexes, all these indexes are restored to the Elasticsearch cluster.

    After you call the `restore` operation, the system immediately returns a response and restores the index at the backend. If you want the system to return a response after the index is restored, specify the `wait_for_completion` parameter.

    ```
    POST _snapshot/my_backup/snapshot_1/_restore?wait_for_completion=true
    ```

-   Restore specified indexes and rename the indexes. If you only want to verify or process the data in indexes and do not want to overwrite the data, use this method to restore the indexes.

    ```
    POST /_snapshot/my_backup/snapshot_1/_restore
    {
     "indices": "index_1", <1>
     "rename_pattern": "index_(.+)", <2>
     "rename_replacement": "restored_index_$1" <3>
    }
    ```

    In this example, the `index_1` index is restored to your Elasticsearch cluster and renamed `restored_index_1`.

    -   <1\>: The system restores only the `index_1` index from the snapshot.
    -   <2\>: The system searches for the index that is being restored and matches the index name with the provided index pattern.
    -   <3\>: The system renames the matched index.

## Monitor index restoration progress

**Note:** Restoring data from a repository applies the existing restoration mechanism in Elasticsearch. Restoring shards from a repository is the same as restoring data from a node.

You can call the recovery operation to monitor the progress of an index restoration task.

-   Monitor the restoration of a specified index.

    ```
    GET restored_index_3/_recovery
    ```

    The recovery operation is a general-purpose operation. It can be used to query the status of the shards that are being transmitted to your cluster.

-   Monitor the restoration of all indexes on the cluster. This may include shards that are irrelevant to the restoration process.

    ```
    GET /_recovery/
    ```

    Sample response:

    ```
    {
    "restored_index_3" : {
     "shards" : [ {
       "id" : 0,
       "type" : "snapshot", <1>
       "stage" : "index",
       "primary" : true,
       "start_time" : "2014-02-24T12:15:59.716",
       "stop_time" : 0,
       "total_time_in_millis" : 175576,
       "source" : { <2>
         "repository" : "my_backup",
         "snapshot" : "snapshot_3",
         "index" : "restored_index_3"
       },
       "target" : {
         "id" : "ryqJ5lO5S4-lSFbGntkEkg",
         "hostname" : "my.fqdn",
         "ip" : "10.0. **.**",
         "name" : "my_es_node"
       },
       "index" : {
         "files" : {
           "total" : 73,
           "reused" : 0,
           "recovered" : 69,
           "percent" : "94.5%" <3>
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

    -   <1\>: The `type` parameter indicates the type of restoration. The value `snapshot` indicates that the shard is being restored from a snapshot.
    -   <2\>: The `source` parameter indicates the source snapshot and repository.
    -   <3\>: The `percent` parameter indicates the progress of the restoration task. The value `94.5%` indicates that 94.5% of shard files are restored.
    The response lists all indexes that are being restored and the shards in these indexes. Each shard contains statistics about the start or end time, duration, restoration progress, and transmitted bytes.


## Cancel index restoration

To cancel index restoration, you only need to delete the indexes that are being restored. A restoration process is a shard restoration process. You can call the delete operation to modify the status of a cluster and cancel index restoration. Example:

```
DELETE /restored_index_3
```

If you run the preceding command when the `restored_index_3` index is being restored, the system stops the restoration and deletes the data that has been restored to your cluster. For more information, see [Snapshot And Restore](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/modules-snapshots.html).

## Delete a snapshot

You can specify a repository name and a snapshot name in a `delete` request to delete the specified snapshot. Example:

```
DELETE _snapshot/my_backup/snapshot_2
```

**Note:**

-   You can delete snapshots only by calling the delete operation. A snapshot is associated with other backup files. Some of the files may also be used by other snapshots. The delete operation does not delete files that are still being used by other snapshots. It deletes only the files that are associated with deleted snapshots and are no longer used by other snapshots.
-   If you choose to manually delete a snapshot, you may delete files that are used by other snapshots. This can cause data loss.

