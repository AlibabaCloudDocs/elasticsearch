---
keyword: [跨集群oss仓库, 不同es集群间数据恢复]
---

# 设置跨集群OSS仓库

通过设置跨集群OSS仓库，您可以将已进行了自动快照备份的源Elasticsearch实例仓库中的数据，恢复到目标Elasticsearch实例中。例如有两个6.7.0版本的Elasticsearch实例，ID分别为es-cn-a和es-cn-b，其中es-cn-a已经开通了自动快照备份功能，且已经进行过一次快照。如果es-cn-b想从es-cn-a的自动快照恢复数据，那么需要设置跨集群OSS仓库。

源端实例与目标端实例需要满足以下条件：

-   相同区域。
-   归属于相同账号。
-   源端实例的版本低于或等于目标端实例的版本。

    如果源端和目标端实例的版本都是商业版6.7.0，请确保两个实例的内核版本都是最新或者目标端的内核版本比源端高。

    **说明：**

    -   **跨集群OSS仓库设置**功能只支持高版本的实例引用相同版本或低版本的仓库，不支持低版本实例引用高版本仓库。
    -   当高版本的实例引用低版本实例的仓库时，需要注意高版本的实例对低版本实例的数据格式可能存在不兼容的情况。例如，从5.5.3版本的实例恢复数据到6.7.0版本的实例，对于单类型的索引，5.5.3版本的实例支持恢复数据到6.7.0版本；对于多类型索引，由于5.5.3版本的实例只支持多类型索引，而6.7.0版本不支持多类型索引，所以恢复可能会出现问题。

## 添加OSS仓库引用

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**数据备份**。

5.  在**跨集群OSS仓库设置**区域，单击**立即创建**。

    **说明：** 如果不是首次添加仓库引用，需要单击**创建OSS引用仓库**。

6.  在**创建OSS引用仓库**页面，选择实例。

    **说明：** 所选实例与当前实例需要满足上文的前提条件。

7.  单击**确认**。

    添加成功后，被引用的实例显示在当前页面，并显示引用仓库的状态。

    ![添加引用仓库成功状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1107458061/p63593.png)

    **说明：** 由于仓库列表是通过访问对应实例获取到的，因此当实例在变更中、不健康或者负载特别高时，可能无法获取仓库情况。此时，您可以在Kibana控制台中，执行`GET _snapshot`命令，获取所有仓库的地址。具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

8.  恢复索引。

    跨集群OSS仓库设置功能只是实现了实例间仓库的引用，并不会自动进行数据的恢复。您可以按照需求在目标Elasticsearch实例的Kibana控制台上执行对应命令，才能恢复需要的索引数据。例如，从实例es-cn-a恢复file-2019-08-25索引，操作步骤如下：

    1.  登录目标Elasticsearch实例的Kibana控制台。

        具体步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  在左侧导航栏，单击**Dev Tools**。

    3.  在**Console**中，执行以下命令，查询指定实例仓库中的所有快照信息。

        ```
        GET /_cat/snapshots/aliyun_snapshot_from_es-cn-a?v
        ```

        该请求会返回指定仓库下所存储的所有快照信息。

        ![返回该仓库下所存储的所有快照信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1056359951/p63598.png)

        **说明：** `aliyun_snapshot_from_es-cn-a`为[添加OSS仓库引用](#section_zf4_nr6_ie2)中的引用仓库名称。

    4.  根据上一步获取的快照id，执行以下命令恢复该快照下的指定索引。

        **说明：**

        -   请确保指定索引在目标Elasticsearch实例中处于关闭状态，或者没有该索引。如果在执行恢复索引命令之前，目标Elasticsearch实例中已有相同名称的索引，并且处于开启状态，那么在执行恢复索引命令时会报错。
        -   恢复`.`开头的系统索引可能会导致Kibana访问失败，建议不要恢复系统索引数据。
        -   恢复单个索引

            ```
            POST _snapshot/aliyun_snapshot_from_es-cn-a/es-cn-a_20190705220000/_restore 
             {"indices": "file-2019-08-25"}
            ```

        -   恢复多个索引

            ```
            POST _snapshot/aliyun_snapshot_from_es-cn-a/es-cn-a_20190705220000/_restore
             {"indices": "kibana_sample_data_ecommerce,kibana_sample_data_logs"}
            ```

        -   恢复所有索引（除过`.`开头的系统索引）

            ```
            POST _snapshot/aliyun_snapshot_from_es-cn-a/es-cn-a_20190705220000/_restore 
            {"indices":"*,-.monitoring*,-.security*,-.kibana*","ignore_unavailable":"true"}
            ```


