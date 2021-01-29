# 从AWS迁移Elasticsearch索引至阿里云

本文介绍如何将索引数据从AWS Elasticsearch迁移到阿里云Elasticsearch中。

## 背景信息

本次Elasticsearch索引迁移方案的参考架构如下。

![ES索引迁移方案的参考架构图](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/72927/intl_en/1527686872268/01.png)

## 相关概念

-   Elasticsearch：一个分布式的RESTful风格的搜索与分析引擎，适用于各种应用场景。作为Elastic Stack的核心，Elasticsearch可以集中存储您的数据，并对数据进行搜索分析。
-   Kibana：您可以使用Kibana对Elasticsearch数据进行可视化搜索分析。
-   亚马逊Elasticsearch服务（简称AWS Elasticsearch）： 一项完全托管的服务，提供了各种易于使用的Elasticsearch API和实时分析功能，还可以实现生产工作负载需要的可用性、可扩展性和安全性。您可以使用Amazon Elasticsearch Service轻松部署、保护、操作和扩展Elasticsearch，以便进行日志分析、全文搜索和应用程序监控等工作。
-   阿里云Elasticsearch服务： 提供基于开源Elasticsearch服务，致力于数据分析、数据搜索等场景服务。在开源Elasticsearch基础上提供企业级权限管控、安全监控告警、自动报表生成等功能。本文涉及的操作主要在阿里云Elasticsearch服务的中国站上进行。
-   快照和恢复（Snapshot and Restore）：您可以使用快照和恢复功能，在远程存储库（如共享文件系统、S3或HDFS）中，创建各个索引或整个集群的快照。创建后的快照可以被恢复到对应版本的Elasticsearch中。
    -   在5.x中创建的索引快照可以恢复到6.x。
    -   在2.x中创建的索引快照可以恢复到5.x。
    -   在1.x中创建的索引快照可以恢复到2.x。

## 解决方案概述

您可以通过以下步骤来迁移索引数据：

1.  创建基线索引。
    1.  创建一个快照存储库，并将其与Amazon Simple Storage Service （AWS S3）存储空间相关联。
    2.  为要迁移的索引创建第一个完整的快照。

        该快照会自动存储在步骤一中创建的AWS S3存储空间中。

    3.  在阿里云侧创建一个对象存储服务OSS（Object Storage Service）存储空间，并将其注册到阿里云Elasticsearch实例的快照存储库中。
    4.  使用OSS Import工具将AWS S3存储空间中的数据提取到阿里云OSS存储空间中。
    5.  将此完整快照恢复到阿里云Elasticsearch实例。
2.  定期处理增量快照 。

    重复以上步骤处理增量快照并进行恢复。

3.  确定最终快照，进行服务切换。
    1.  停止可能会修改索引数据的服务。
    2.  创建AWS Elasticsearch实例的最终增量快照。
    3.  将最终增量快照迁移至OSS，然后恢复到阿里云Elasticsearch实例中。
    4.  进行服务切换，在阿里云Elasticsearch实例中，查看迁移成功的数据。

## 前提条件

您已完成以下操作：

-   创建AWS Elasticsearch实例，版本号为5.5.2，区域为新加坡。

    具体操作步骤请参见[创建Amazon Elasticsearch域](https://docs.amazonaws.cn/elasticsearch-service/latest/developerguide/es-gsg-create-domain.html)。

-   创建阿里云Elasticsearch实例，版本号为5.5.3，区域为杭州。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/Elasticsearch/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   创建OSS Bucket。

    本文创建的Bucket区域为华东1（杭州）、存储类型为标准存储、读写权限为私有，其他参数保持默认，具体操作步骤请参见[创建存储空间](/intl.zh-CN/快速入门/控制台快速入门/创建存储空间.md)。

-   准备待迁移的索引，示例索引名称为`movies`。

## 在AWS中创建手动快照的前提条件

AWS Elasticsearch每天会自动为一个域中的主要索引分片创建快照，并将这些快照存储在预配置的AWS S3存储空间中。这些快照会保留14天，您无需额外付费。此外，您还可以使用这些快照来恢复域，但是这些自动快照不能被迁移到新域。如果要迁移，您必须使用存储在自己的存储库（S3存储空间）中的手动快照。手动快照将收取标准S3费用。

您需要使用Amazon Identity and Access Management（IAM）和AWS S3才能手动创建和恢复索引快照。创建快照之前，请确保已满足以下条件。

|前提条件|描述|
|----|--|
|创建AWS S3存储空间|存储AWS Elasticsearch域的手动快照。|
|创建IAM角色|为AWS Elasticsearch服务授权。在给该角色添加信任关系时，必须在`Principal`语句中指定AWS Elasticsearch服务。使用AWS Elasticsearch注册您的快照存储库时，也需要使用该IAM角色。只有具有该角色访问权限的IAM用户才可以注册快照存储库。|
|创建IAM策略|指定IAM角色可以对S3存储空间执行的操作。该策略必须添加给为AWS Elasticsearch服务授权的IAM角色。您需要在该策略的Resource语句中指定S3存储空间。|

-   创建S3存储空间

    创建一个AWS S3存储空间来存储手动快照，并记录其Amazon资源名称（ARN）。该ARN在以下场景中会用到：

    -   用于IAM角色添加IAM策略的Resource语句。
    -   用于注册快照存储库的Python客户端。
    AWS S3存储空间的ARN示例如下。

    ```
    arn:aws:s3:::eric-es-index-backups
    ```

-   创建IAM角色

    确保已经创建了一个IAM角色，且在其信任关系中的`Service`语句中指定该角色的服务类型为AWS Elasticsearch服务（es.amazonaws.com），如下所示。

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

    您可以在AWS IAM控制台查看信任关系的详细信息。

    ![查看信任关系](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6302659951/p87368.png)

    **说明：** 在IAM控制台创建AWS服务角色时，**Select role type**下拉列表中不包含AWS Elasticsearch。 但是，您可以先选择**Amazon EC2**，按照提示完成角色创建，然后将`ec2.amazonaws.com`修改为`es.amazonaws.com`。

-   创建IAM策略

    创建IAM策略，并将IAM策略添加给IAM角色。该策略指定存储AWS Elasticsearch域的S3存储空间。以下示例指定了存储空间`eric-es-index-backups`的ARN。

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

    1.  将策略内容复制到编辑策略区域。

        ![策略编辑区域](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6302659951/p87369.png)

    2.  检查策略是否正确。

        ![Policy Summary](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6302659951/p87370.png)

    3.  为IAM角色添加IAM策略。

        ![为IAM角色添加IAM策略](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6302659951/p87373.png)


## 步骤一：注册手动快照存储库

您必须通过AWS Elasticsearch服务注册快照存储库后，才能创建手动索引快照。创建手动索引快照前，需要先为IAM角色的信任关系中指定的用户或角色签发您的AWS请求，详情请参见[在AWS中创建手动快照的前提条件](#section_vsd_f3p_fhb)。

**说明：** 由于curl命令不支持AWS请求签名，因此不能使用curl命令注册快照存储库。请使用[示例Python客户端](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/72927/intl_en/1527694314531/Sample%20Python%20Client.docx)注册您的快照存储库。

1.  下载[示例Python客户端](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/72927/intl_en/1527694314531/Sample%20Python%20Client.docx)文件。
2.  修改示例Python客户端文件。

    修改文件中标黄的值，填入实际匹配的值。修改完成后，复制示例Python客户端文件中的内容至Python文件中，并命名为snapshot.py。

    示例Python客户端文件中的参数说明如下。

    |变量名|描述|
    |---|--|
    |region|创建快照存储库所在的AWS地域。|
    |host|AWS Elasticsearch域的访问地址。|
    |aws\_access\_key\_id|IAM凭证ID。|
    |aws\_secret\_access\_key|IAM凭证Key。|
    |path|快照存储库的路径。|
    |data|必须包含上文[在AWS中创建手动快照的前提条件](#section_vsd_f3p_fhb)中，为IAM角色创建的S3存储空间的名字和ARN。 **说明：**

    -   如果要为快照存储库启用S3托管密钥的服务器端加密，请将 `"server_side_encryption": true`添加到settings JSON中。
    -   如果S3存储空间在ap-southeast-1地域，请使用`"endpoint": "s3.amazonaws.com"`替代`"region": "ap-southeast-1"`。 |

3.  安装Amazon Web Services Library boto-2.48.0。

    上文的示例Python客户端，要求您在注册快照存储库的计算机上安装boto软件包的2.x版本。

    ```
    # wget https://pypi.python.org/packages/66/e7/fe1db6a5ed53831b53b8a6695a8f134a58833cadb5f2740802bc3730ac15/boto-2.48.0.tar.gz#md5=ce4589dd9c1d7f5d347363223ae1b970 
    # tar zxvf boto-2.48.0.tar.gz
    # cd boto-2.48.0
    # python setup.py install
    ```

4.  执行Python客户端，注册快照存储库。

    ```
    # pyth
    on snapshot.py
    ```

5.  进入对应AWS Elasticsearch的Kibana控制台，在**Dev Tools**页面的**Console**中，执行以下命令，查看请求结果。

    ```
    GET _snapshot
    ```

    ![查看请求结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87377.png)


## 步骤二：创建首次快照并恢复

1.  在AWS Elasticsearch上手动创建快照。

    **说明：** 以下命令可在Kibana控制台上执行，也可以在Linux或者Mac OSX命令行中使用curl命令来执行。

    -   为在`eric-snapshot-repository`存储库中的`movies`索引，创建名称为`snapshot_movies_1`的快照。

        ```
        PUT _snapshot/eric-snapshot-repository/snapshot_movies_1
        {
        "indices": "movies"
        }
        ```

    -   查看快照状态。

        ```
        GET _snapshot/ eric-snapshot-repository/snapshot_movies_1
        ```

        ![查看快照状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87378.png)

    -   在AWS S3控制台中，查看快照文件。

        ![瞎看快照文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87381.png)

2.  从AWS S3提取快照数据至阿里云OSS。

    详细操作方法请参见[从AWS S3上的应用无缝切换至OSS](/intl.zh-CN/最佳实践/数据迁移/从AWS S3上的应用无缝切换至OSS.md)。

    数据提取后，在OSS控制台中查看存储的快照数据。

    ![OSS快照数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87382.png)

3.  还原快照至阿里云Elasticsearch实例。
    1.  创建快照存储库。

        进入目标阿里云Elasticsearch实例的Kibana控制台（[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)），在**Dev Tools**页面的**Console**中，执行如下命令创建一个同名的快照存储库。

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

    2.  查看名称为`snapshot_movies_1`的快照状态。

        ```
        GET _snapshot/eric-snapshot-repository/snapshot_movies_1
        ```

        ![查看快照](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87387.png)

        **说明：** 请记录此快照操作的起始时间和结束时间。当您使用阿里云OssImport迁移工具迁移增量快照数据时，此记录会被用到。例如：

        -   “start\_time\_in\_millis”: 1519786844591
        -   “end\_time\_in\_millis”: 1519786846236
4.  恢复快照。

    执行以下命令，查看`movies`索引的可用性。

    ```
    POST _snapshot/eric-snapshot-repository/snapshot_movies_1/_restore
    {
        "indices": "movies"
    }
    GET movies/_recovery
    ```

    执行成功后，可以看到`movies`索引中存在三组数据，且与AWS Elasticsearch实例中的数据相同。

    ![查看ES索引数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87388.png)


## 步骤三：创建末次快照并恢复

1.  在AWS Elasticsearch的`movies`索引中插入数据。

    `movies`索引中已存在三组数据，您还需插入另两组数据。

    ![插入另外两条数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87389.png)

    使用`GET movies/_count`命令，可查看索引数据量。

2.  手动创建另一个快照。

    执行以下命令手动创建快照，详情请参见上文的[在AWS Elasticsearch上手动创建快照](#section_ic4_3jp_fhb)步骤。

    ```
    PUT _snapshot/eric-snapshot-repository/snapshot_movies_2
    {
    "indices": "movies"
    }
    ```

    创建成功后，执行以下命令查看快照状态。

    ```
    GET _snapshot/eric-snapshot-repository/snapshot_movies_2
    ```

    查看S3存储空间中列出的文件。

    ![查看S3空间文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87392.png)

3.  从AWS S3提取增量快照数据至阿里云OSS。

    您可以使用OSSImport工具从AWS S3迁移数据至阿里云OSS。目前有两个快照文件存储在S3存储空间里，可以通过修改配置文件local\_job.cfg中的`isSkipExistFile`变量来迁移新的文件。

    `isSkipExistFile`表示数据迁移期间是否跳过现有对象，为布尔类型，默认值为false。如果设置为true，则根据`size`和`LastModifiedTime`跳过对象；如果设置为false，则覆盖现有对象。当`jobType`设置为`audit`时，此选项无效。

    迁移工作完成后，您可以看到新的文件已被迁移至OSS中。

    ![新文件迁移到OSS中](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87402.png)

4.  恢复增量快照。

    恢复增量快照详细步骤请参见上文的[步骤二：创建首次快照并恢复](#section_ic4_3jp_fhb)章节中的恢复快照步骤。但是首先需要关闭`movies`索引，然后再恢复快照。快照恢复后可以再次打开`movies`索引。

    -   关闭`movies`索引

        ```
        POST /movies/_close
        ```

    -   查看`movies`索引状态

        ```
        GET movies/_stats
        ```

    -   恢复增量快照

        ```
        POST _snapshot/eric-snapshot-repository/snapshot_movies_2/_restore
        {
            "indices": "movies"
        }
        ```

    -   打开`movies`索引

        ```
        POST /movies/_open
        ```

    恢复快照步骤完成后，可以看到`movies`索引中文档数量为`5`，与AWS Elasticsearch实例中的文档数量相同。

    ![恢复快照结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7302659951/p87394.png)


## 总结

您可以通过创建和恢复快照的方法，将AWS Elasticsearch的索引数据迁移至阿里云Elasticsearch索引中。此方案要求先关闭需要迁移的AWS Elasticsearch索引，以防止迁移期间的写入和请求。

相关文档：

-   [Elasticsearch官方文档](https://www.elastic.co/products/elasticsearch)
-   [Snapshot module](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)
-   [Working with Amazon Elasticsearch Service Index Snapshots](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains-snapshots.html)
-   [从AWS S3上的应用无缝切换至OSS](/intl.zh-CN/最佳实践/数据迁移/从AWS S3上的应用无缝切换至OSS.md)
-   [ossimport说明及配置](/intl.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md)

