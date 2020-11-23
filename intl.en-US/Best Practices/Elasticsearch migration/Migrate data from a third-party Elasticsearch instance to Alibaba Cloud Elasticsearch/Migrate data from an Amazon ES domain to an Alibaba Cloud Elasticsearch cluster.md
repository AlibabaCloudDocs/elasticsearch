# Migrate data from an Amazon ES domain to an Alibaba Cloud Elasticsearch cluster

This topic describes how to migrate data from an Amazon Elasticsearch Service \(Amazon ES\) domain to an Alibaba Cloud Elasticsearch cluster.

## Background information

The following figure shows the reference architecture for the migration.

![Reference architecture for Elasticsearch data migration](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/72927/intl_en/1527686872268/01.png)

## Terms

-   Elasticsearch: a distributed, RESTful search and analytics engine designed for various scenarios. As the core of the Elastic Stack, Elasticsearch stores your data in a centralized manner and searches for and analyzes data.
-   Kibana: provides a visual interface for you to search for and analyze data.
-   Amazon ES: a fully managed service that offers easy-to-use Elasticsearch API operations and real-time analytics capabilities. This service also provides the availability, scalability, and security that are required for production workloads. You can use Amazon ES to easily deploy, protect, manage, and scale Elasticsearch clusters for scenarios such as log analytics, full-text search, and application monitoring.
-   Alibaba Cloud Elasticsearch: It is designed based on open source Elasticsearch for scenarios such as data analytics and searches. It provides enterprise-grade access control, automated reporting, and security monitoring and alerting.
-   Snapshot and restore: You can store snapshots of individual indexes or an entire cluster in a remote repository like a shared file system, such as Amazon Simple Storage Service \(Amazon S3\) or HDFS. The snapshots can be used to restore data. However, the data can be restored only to Elasticsearch clusters of specific versions:
    -   Data in a snapshot created in an Elasticsearch 5.x cluster can be restored to an Elasticsearch 6.x cluster.
    -   Data in a snapshot created in an Elasticsearch 2.x cluster can be restored to an Elasticsearch 5.x cluster.
    -   Data in a snapshot created in an Elasticsearch 1.x cluster can be restored to an Elasticsearch 2.x cluster.

## Migration plan

To migrate data from an Amazon ES domain to an Alibaba Cloud Elasticsearch cluster, perform the following steps:

1.  Create a baseline index.
    1.  Create a snapshot repository and associate it with an S3 bucket.
    2.  Create the first snapshot for the index whose data you want to migrate. The first snapshot is a full snapshot.

        This snapshot is automatically stored in the S3 bucket.

    3.  Create an Object Storage Service \(OSS\) bucket on Alibaba Cloud, and register it with the snapshot repository of your Alibaba Cloud Elasticsearch cluster.
    4.  Use ossimport to transfer the full snapshot from the S3 bucket to the OSS bucket.
    5.  Restore data from the full snapshot to your Alibaba Cloud Elasticsearch cluster.
2.  Process incremental snapshots on a regular basis.

    Repeat the preceding steps to restore data from incremental snapshots.

3.  Identify the final snapshot and perform a service switchover.
    1.  Stop services that may modify index data.
    2.  Create the final snapshot for your Amazon ES domain.
    3.  Transfer the final snapshot to your OSS bucket. Then, restore data from the snapshot to your Alibaba Cloud Elasticsearch cluster.
    4.  Perform a service switchover to the cluster.

## Prerequisites

You have completed the following operations:

-   Create an Amazon ES 5.5.2 domain in the Asia Pacific \(Singapore\) region.

    For more information, see [Create an Amazon ES domain](https://docs.amazonaws.cn/elasticsearch-service/latest/developerguide/es-gsg-create-domain.html).

-   Create an Alibaba Cloud Elasticsearch V5.5.3 cluster in the China \(Hangzhou\) region.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

-   Create an OSS bucket.

    In this topic, an OSS bucket is created in the China \(Hangzhou\) region. The storage class of the bucket is Standard, and the access control list \(ACL\) of the bucket is Private. Default settings are used for other parameters. For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

-   Prepare the index whose data you want to migrate. The `movies` index is used in this topic.

## Prerequisites for creating manual snapshots in an Amazon ES domain

Amazon ES automatically creates snapshots for the primary index shards in a domain every day and stores them in a pre-configured S3 bucket. These snapshots are retained for a maximum of 14 days free of charge. You can use these snapshots to restore data to the domain. However, you cannot use them to migrate data to other domains. To migrate data, you must use manual snapshots stored in your S3 bucket. Standard S3 charges apply to manual snapshots.

To create manual snapshots and restore data from the snapshots, you must use AWS Identity and Access Management \(IAM\) and S3. Before you create snapshots, perform the operations that are listed in the following table.

|Operation|Description|
|---------|-----------|
|Create an S3 bucket|The bucket stores the manual snapshots of your Amazon ES domain.|
|Create an IAM role|The role is used to grant permissions on Amazon ES. When you add a trust relationship for the role, you must specify Amazon ES in the `Principal` element. This role is also required when you register a snapshot repository with Amazon ES. Only IAM users that assume this role can register the snapshot repository.|
|Create an IAM policy|This policy specifies the actions that S3 can perform on your S3 bucket. The policy must be attached to the IAM role that is used to grant permissions on Amazon ES. You must specify your S3 bucket in the Resource element of the policy.|

-   Create an S3 bucket

    You need an S3 bucket to store manual snapshots. Take note of its Amazon Resource Name \(ARN\). The ARN is used by the following items:

    -   Resource element in the IAM policy that is attached to your IAM role
    -   Python client that is used to register a snapshot repository
    The following example shows the ARN of an S3 bucket:

    ```
    arn:aws:s3:::eric-es-index-backups
    ```

-   Create an IAM role

    You must have an IAM role, for which Amazon ES \(es.amazonaws.com\) is specified in the `Service` element in its trust relationship. Example:

    ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "",
          "Effect": "Allow",
          "Principal": {
            "Service": "es.amazonaws.com"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
    ```

    You can view the trust relationship details in the AWS IAM console.

    ![View trust relationships](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5357359951/p87368.png)

    **Note:** When you create a role in the IAM console, Amazon ES is not included in the **Select role type** drop-down list. You can select **Amazon EC2** from the drop-down list and create the role as prompted. Then, change `ec2.amazonaws.com` in the trust relationship of the role to `es.amazonaws.com`.

-   Create an IAM policy

    You must attach an IAM policy to the IAM role. The policy specifies the S3 bucket that is used to store the manual snapshots of your Amazon ES domain. The following example specifies the ARN of the `eric-es-index-backups` bucket:

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "s3:ListBucket"
                ],
                "Effect": "Allow",
                "Resource": [
                    "arn:aws:s3:::eric-es-index-backups"
                ]
            },
            {
                "Action": [
                    "s3:GetObject",
                    "s3:PutObject",
                    "s3:DeleteObject"
                ],
                "Effect": "Allow",
                "Resource": [
                    "arn:aws:s3:::eric-es-index-backups/*"
                ]
            }
        ]
    }
    ```

    1.  Copy the policy content to the Edit policy section.

        ![Edit policy section](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5357359951/p87369.png)

    2.  Check whether the specified policy is correct.

        ![Policy Summary](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5357359951/p87370.png)

    3.  Attach the policy to the role.

        ![Attach an IAM policy to an IAM role](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5357359951/p87373.png)


## Step 1: Register a manual snapshot repository

You can create manual snapshots only after you register a snapshot repository with Amazon ES. Before you create manual snapshots, sign your AWS request to the user or role specified in the trust relationship of the IAM role. For more information, see [Prerequisites for creating manual snapshots in an Amazon ES domain](#section_vsd_f3p_fhb).

**Note:** You cannot use a curl command to register a snapshot repository because the command does not support AWS request signing. Instead, use the [sample Python client](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/72927/intl_en/1527694314531/Sample%20Python%20Client.docx) to register a snapshot repository.

1.  Download the [Sample Python Client](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/72927/intl_en/1527694314531/Sample%20Python%20Client.docx) file.
2.  Modify the file.

    Change the values highlighted in yellow in the file based on actual conditions. Then, copy the content into a Python file named snapshot.py.

    The following table describes the parameters in the Sample Python Client file.

    |Parameter|Description|
    |---------|-----------|
    |region|The AWS region where the snapshot repository is created.|
    |host|The endpoint of your Amazon ES domain.|
    |aws\_access\_key\_id|The ID of your IAM credential.|
    |aws\_secret\_access\_key|The key of your IAM credential.|
    |path|The path of the snapshot repository.|
    |data|The value must include the name and ARN of the S3 bucket for the IAM role that you created in [Prerequisites for creating manual snapshots in an Amazon ES domain](#section_vsd_f3p_fhb). **Note:**

    -   If you want to enable server-side encryption with S3-managed keys for the snapshot repository, add `"server_side_encryption": true` to the settings JSON array.
    -   If the S3 bucket resides in the ap-southeast-1 region, replace `"region": "ap-southeast-1"` with `"endpoint": "s3.amazonaws.com"`. |

3.  Install Amazon Web Services Library boto-2.48.0.

    The preceding sample Python client requires that you install the boto package of version 2.x on the computer where you register your snapshot repository.

    ```
    # wget https://pypi.python.org/packages/66/e7/fe1db6a5ed53831b53b8a6695a8f134a58833cadb5f2740802bc3730ac15/boto-2.48.0.tar.gz#md5=ce4589dd9c1d7f5d347363223ae1b970 
    # tar zxvf boto-2.48.0.tar.gz
    # cd boto-2.48.0
    # python setup.py install
    ```

4.  Run the Python client to register the snapshot repository.

    ```
    # pyth
    on snapshot.py
    ```

5.  Log on to the Kibana console of your AWS ES domain. In the left-side navigation pane, click **Dev Tools**. On the **Console** tab of the page that appears, run the following command to view the registration result:

    ```
    GET _snapshot
    ```

    ![View the registration result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6357359951/p87377.png)


## Step 2: Create the first snapshot and restore data from the snapshot

1.  Create a snapshot in your Amazon ES domain.

    **Note:** You can run the following commands in the Kibana console or by using curl commands in the Linux or Mac OS X command line interface \(CLI\).

    -   Create a snapshot named `snapshot_movies_1` for the `movies` index in the `eric-snapshot-repository` snapshot repository.

        ```
        PUT _snapshot/eric-snapshot-repository/snapshot_movies_1
        {
        "indexes": "movies"
        }
        ```

    -   View snapshot status.

        ```
        GET _snapshot/ eric-snapshot-repository/snapshot_movies_1
        ```

        ![View snapshot status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6357359951/p87378.png)

    -   In the S3 console, view snapshot objects.

        ![View snapshot objects](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6357359951/p87381.png)

2.  Transfer the created snapshot from your S3 bucket to your OSS bucket.

    For more information, see [Migrate data from Amazon S3 to Alibaba Cloud OSS](/intl.en-US/Best Practices/Migrate data to OSS/Migrate data from Amazon S3 to Alibaba Cloud OSS.md).

    After the snapshot is transferred, view the snapshot in the OSS console.

    ![Snapshot stored in OSS](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6357359951/p87382.png)

3.  Restore data from the snapshot to your Alibaba Cloud Elasticsearch cluster.
    1.  Create a snapshot repository.

        Log on to the Kibana console of your Elasticsearch cluster. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md). Then, in the left-side navigation pane, click **Dev Tools**. On the **Console** tab of the page that appears, run the following command to create a snapshot repository. The name of the snapshot repository must be the same as that of the snapshot repository registered with Amazon ES.

        ```
        PUT _snapshot/eric-snapshot-repository
        {
        "type": "oss",
        "settings": {
                    "endpoint": "http://oss-cn-hangzhou-internal.aliyuncs.com",  
                    "access_key_id": "your AccessKeyID",
                    "secret_access_key": "your AccessKeySecret ",
                    "bucket": "eric-oss-aws-es-snapshot-s3",
                    "compress": true
              }
        }
        ```

    2.  View the status of the snapshot named `snapshot_movies_1`.

        ```
        GET _snapshot/eric-snapshot-repository/snapshot_movies_1
        ```

        ![View snapshots](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6357359951/p87387.png)

        **Note:** Take note of the start time and end time of the snapshot creation operation. This record is used when you use ossimport to migrate data in incremental snapshots. Example:

        -   "start\_time\_in\_millis": 1519786844591
        -   "end\_time\_in\_millis": 1519786846236
4.  Restore data from the snapshot.

    Run the following command to check the availability of the `movies` index:

    ```
    POST _snapshot/eric-snapshot-repository/snapshot_movies_1/_restore
    {
        "indexes": "movies"
    }
    GET movies/_recovery
    ```

    After the command is successfully executed, you can view three sets of data in the `movies` index. In addition, the data is the same as that in the Amazon ES domain.

    ![View index data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6357359951/p87388.png)


## Step 3: Create the final snapshot and restore data from the snapshot

1.  Insert data into the `movies` index in your Amazon ES domain.

    The `movies` index contains three sets of data. Insert another two sets of data.

    ![Insert another two sets of data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6357359951/p87389.png)

    You can run the `GET movies/_count` command to view the data volume of the index.

2.  Create a snapshot.

    Run the following command to create a snapshot. For more information, see [Create a snapshot in your Amazon ES domain](#section_ic4_3jp_fhb).

    ```
    PUT _snapshot/eric-snapshot-repository/snapshot_movies_2
    {
    "indices": "movies"
    }
    ```

    After the snapshot is created, run the following command to view the status of the snapshot:

    ```
    GET _snapshot/eric-snapshot-repository/snapshot_movies_2
    ```

    View objects in your S3 bucket.

    ![View objects in your S3 bucket](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7357359951/p87392.png)

3.  Transfer the snapshot from your S3 bucket to your OSS bucket.

    You can use ossimport to transfer the snapshot. The S3 bucket stores two snapshot objects. You can change the value of the `isSkipExistFile` variable in the local\_job.cfg file to migrate the incremental snapshot object.

    The `isSkipExistFile` variable indicates whether existing objects are skipped during data migration. The value of this variable is of the Boolean type. The default value is false. If you set the value to true, objects are skipped based on the `size` and `LastModifiedTime` settings. If you set the value to false, existing objects are overwritten. If `jobType` is set to `audit`, this variable is invalid.

    Then, you can view the incremental snapshot object in the OSS bucket.

    ![Incremental snapshot object in the OSS bucket](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7357359951/p87402.png)

4.  Restore data from the incremental snapshot.

    For more information, see the "[Step 2: Create the first snapshot and restore data from the snapshot](#section_ic4_3jp_fhb)" section. Before you restore data, you must disable the `movies` index. After the restoration, you can enable the index.

    -   Disable the `movies` index

        ```
        POST /movies/_close
        ```

    -   View the status of the `movies` index

        ```
        GET movies/_stats
        ```

    -   Restore data from the snapshot

        ```
        POST _snapshot/eric-snapshot-repository/snapshot_movies_2/_restore
        {
            "indexes": "movies"
        }
        ```

    -   Enable the `movies` index

        ```
        POST /movies/_open
        ```

    After data is restored from the snapshot, the number of documents in the `movies` index of your Elasticsearch cluster is `5`. This number is the same as that in the index of your Amazon ES domain.

    ![Data restoration result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7357359951/p87394.png)


## Summary

You can use the snapshot and restore feature to migrate data from an Amazon ES domain to an Alibaba Cloud Elasticsearch cluster. This feature requires that you disable the index whose data you want to migrate to avoid requests and write operations during the migration.

References:

-   [Open source Elasticsearch documentation](https://www.elastic.co/products/elasticsearch)
-   [Snapshot module](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)
-   [Working with Amazon Elasticsearch Service Index Snapshots](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains-snapshots.html)
-   [Migrate data from Amazon S3 to Alibaba Cloud OSS](/intl.en-US/Best Practices/Migrate data to OSS/Migrate data from Amazon S3 to Alibaba Cloud OSS.md)
-   [ossimport description and configuration](/intl.en-US/Tools/ossimport/Architectures and configurations.md)

