---
keyword: [OSS迁移自建es数据, es数据迁移]
---

# 通过OSS将自建Elasticsearch数据迁移至阿里云

当您需要将自建Elasticsearch数据迁移至阿里云Elasticsearch时，可以使用OSS快照的方式进行迁移。即使用Elasticsearch的snapshot API，创建自建Elasticsearch数据的快照，并存储到OSS中，然后从OSS将快照数据恢复到阿里云Elasticsearch中。本文介绍具体的实现方法。

通过OSS将自建Elasticsearch数据迁移至阿里云Elasticsearch，适用于自建Elasticsearch数据量比较大的场景，简单流程如下。

![操作流程](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9202659951/p113376.png)

## 操作流程

1.  [准备工作](#section_crk_fhz_uvr)

    完成搭建自建Elasticsearch集群、创建OSS Bucket、创建阿里云Elasticsearch集群。

2.  [步骤一：安装elasticsearch-repository-oss插件](#section_nx5_kz4_1xs)

    在自建Elasticsearch各节点中安装elasticsearch-repository-oss插件，插件安装后才可在自建Elasticsearch中创建OSS仓库。

3.  [步骤二：在自建Elasticsearch集群中创建仓库](#section_ftm_9c8_55t)

    使用snapshot API在自建Elasticsearch中创建快照备份仓库。

4.  [步骤三：为指定索引创建快照](#section_euo_abh_8w0)

    为需要迁移的索引创建快照，并将快照备份到已创建的仓库中。

5.  [步骤四：在阿里云Elasticsearch上创建相同仓库](#section_rr1_qrd_wxk)

    在阿里云Elasticsearch的Kibana控制台中，使用snapshot API创建一个与自建Elasticsearch相同的快照备份仓库。

6.  [步骤五：在阿里云Elasticsearch上恢复快照](#section_q2f_ayf_3kk)

    将仓库中已备份的自建Elasticsearch的快照恢复到阿里云Elasticsearch中，完成数据迁移。

7.  [步骤六：查看快照恢复结果](#section_9h6_0ur_tln)

    快照恢复后，查看恢复的索引和索引数据。


## 准备工作

1.  准备自建Elasticsearch集群。

    如果您还没有自建Elasticsearch集群，建议您使用阿里云ECS进行搭建，具体操作步骤请参见[安装并运行Elasticsearch](https://www.elastic.co/guide/cn/elasticsearch/guide/current/running-elasticsearch.html)。

    本文以单节点的Elasticsearch集群为例进行演示，版本为6.7.0。实际生产中您可以购买多个相同专有网络VPC（Virtual Private Cloud）的ECS搭建Elasticsearch集群，购买ECS的具体步骤请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

2.  开通OSS服务，并创建与自建Elasticsearch所在ECS相同区域的Bucket。

    具体操作步骤请参见[开通OSS服务](/cn.zh-CN/控制台用户指南/开通OSS服务.md)和[创建存储空间](/cn.zh-CN/快速入门/控制台快速入门/创建存储空间.md)。

    **说明：** 请创建标准存储类型的OSS Bucket，不支持归档存储类型。

3.  创建目标阿里云Elasticsearch实例，所选区域与您创建的Bucket相同。

    具体操作步骤请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。


## 步骤一：安装elasticsearch-repository-oss插件

1.  连接自建Elasticsearch集群所在的ECS。

    **说明：** 连接ECS的方式请参见[连接Linux实例](/cn.zh-CN/实例/连接实例/使用VNC连接实例/通过密码认证登录Linux实例.md)。

2.  下载elasticsearch-repository-oss插件。

    本文使用6.7.0版本的插件，要求JDK为8.0及以上版本。

    ```
    wget https://github.com/aliyun/elasticsearch-repository-oss/releases/download/v6.7.0/elasticsearch-repository-oss-6.7.0.zip
    ```

3.  将安装包解压到自建Elasticsearch各节点安装路径的plugins目录下。

    ```
    unzip -d /usr/local/elasticsearch-6.7.0/plugins/elasticsearch-repository-oss elasticsearch-repository-oss-6.7.0.zip
    ```

    您也可以使用命令方式安装插件。

    ```
    ./bin/elasticsearch-plugin install file:///usr/local/elasticsearch-repository-oss-6.7.0.zip
    ```

4.  启动自建Elasticsearch集群各节点。

    ```
    cd /usr/local/elasticsearch-6.7.0/bin
    ./elasticsearch -d
    ```


## 步骤二：在自建Elasticsearch集群中创建仓库

连接自建Elasticsearch所在的ECS，执行如下命令创建仓库。

```
curl -H "Content-Type: application/json" -XPUT localhost:9200/_snapshot/es_backup -d' {"type": "oss", "settings": { "endpoint": "http://oss-cn-hangzhou-internal.aliyuncs.com",  "access_key_id": "your_accesskeyid",  "secret_access_key":"your_accesskeysecret", "bucket": "es-backup-es", "compress": true }}'
```

|参数|说明|
|--|--|
|`es_backup`|仓库名称，可自定义。|
|`type`|仓库类型，请设置为`oss`。|
|`endpoint`|OSS Bucket的访问地址，请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)获取。 **说明：** 如果自建Elasticsearch所在ECS与您的OSS在同一区域，请使用内网地址，否则请使用外网地址。 |
|`access_key_id`|创建OSS Bucket的账号的AccessKey ID，获取方式请参见[如何获取AccessKeyId和AccessKeySecret](/cn.zh-CN/常见问题/一般性问题/如何获取AccessKeyId和AccessKeySecret.md)。|
|`secret_access_key`|创建OSS Bucket的账号的AccessKey Secret，获取方式请参见[如何获取AccessKeyId和AccessKeySecret](/cn.zh-CN/常见问题/一般性问题/如何获取AccessKeyId和AccessKeySecret.md)。|
|`bucket`|您创建的OSS Bucket名称。|
|`compress`|是否压缩。|

创建成功后，返回`"acknowledge":true`。

## 步骤三：为指定索引创建快照

在自建Elasticsearch中创建一个快照，用来备份您需要迁移的索引数据。创建快照时，默认会备份所有打开的索引。如果您不想备份系统索引，例如以`.kibana`、`.security`、`.monitoring`等开头的索引，可在创建快照时指定需要备份的索引。

**说明：** 建议您不要备份系统索引，因为系统索引会占用较大空间。

```
curl -H "Content-Type: application/json" -XPUT localhost:9200/_snapshot/es_backup/snapshot_1?pretty -d'
{
"indices": "index1,index2"
}'
```

`index1`和`index2`为您需要备份的索引名称。快照创建成功后，返回`"accepted" : true`。

## 步骤四：在阿里云Elasticsearch上创建相同仓库

1.  登录目标阿里云Elasticsearch的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**。

3.  在**Console**中执行以下命令，创建与自建Elasticsearch相同的仓库。

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


## 步骤五：在阿里云Elasticsearch上恢复快照

参见[步骤四：在阿里云Elasticsearch上创建相同仓库](#section_rr1_qrd_wxk)，在Kibana控制台上执行以下命令，恢复快照中的所有索引（除过`.`开头的系统索引）。

```
POST _snapshot/es_backup/snapshot_1/_restore
{"indices":"*,-.monitoring*,-.security_audit*","ignore_unavailable":"true"}
```

命令执行成功，返回`"accepted" : true`。

以上命令会恢复快照中的所有索引，您也可以选择需要恢复的索引。同时如果阿里云Elasticsearch集群中有同名索引，而您想在不替换现有数据的前提下，恢复旧数据来验证内容，或者处理其他任务，可在恢复过程中重命名索引。

```
POST _snapshot/es_backup/snapshot_1/_restore
{
  "indices":"index1",
  "rename_pattern": "index(.+)",
  "rename_replacement": "restored_index_$1"
}
```

**说明：** 更多快照和恢复命令请参见[手动备份与恢复](/cn.zh-CN/Elasticsearch/数据备份/手动备份与恢复.md)。

## 步骤六：查看快照恢复结果

参见[步骤四：在阿里云Elasticsearch上创建相同仓库](#section_rr1_qrd_wxk)，在Kibana控制台上执行以下命令，查看恢复结果。

-   查看恢复的索引

    ```
    GET /_cat/indices?v
    ```

    ![查看恢复成功的索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9202659951/p113311.png)

-   查看恢复的索引数据

    ```
    GET /index1/_search
    ```

    执行成功后，返回结果如下。

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


