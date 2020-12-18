---
keyword: [es yml配置, 自动创建索引, 删除索引指定名称, Auditlog索引, 开启Watcher]
---

# 修改YML参数配置

通过修改阿里云Elasticsearch实例的YML参数配置，您可以配置允许自动创建索引、删除索引指定名称、配置Auditlog索引，以及开启Watcher和其他配置操作。

## 操作步骤

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**ES集群配置**。

5.  在**ES集群配置**页面，单击**YML文件配置**右侧的**修改配置**。

6.  在**YML文件配置**页面，按照以下说明进行配置。

    ![ES YML参数配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6065240061/p40138.png)

    |参数|说明|
    |--|--|
    |**自动创建索引**|当Elasticsearch实例在接收到新文档后，如果没有对应索引，是否允许系统自动新建索引。自动创建的索引可能不符合您的预期，不建议开启。 对应的YML文件的配置项为`action.auto_create_index`，默认为`false`。 |
    |**删除索引指定名称**|在删除索引时是否需要明确指定索引名称。如果选择**删除或关闭时索引名称支持通配符**，则可以使用通配符进行批量删除索引。索引删除后不可恢复，请谨慎使用此配置。 对应的YML文件的配置项为`action.destructive_requires_name`，默认为`true`。 |
    |**Auditlog索引**|开启后，系统会记录Elasticsearch实例对应的增、删、改、查等操作产生的索引日志，该日志信息会占用您的磁盘空间，同时也会影响性能，不建议开启，请谨慎使用此配置。 **说明：** 阿里云Elasticsearch 7.4.0版本的实例暂不支持配置该参数。

对应的YML文件的配置项为`xpack.security.audit.enabled`，默认为`false`。 |
    |**开启Watcher**|开启后，可使用X-Pack的Watcher功能，请注意定时清理`.watcher-history*`索引，避免占用大量磁盘空间。 对应的YML文件的配置项为`xpack.watcher.enabled`，默认为`false`。 |
    |**其他Configure配置**|支持的部分配置项如下（以下配置项，如果没有标识具体适用于哪个Elasticsearch版本，默认兼容Elasticsearch 5.x、6.x和7.x版本）：     -   [自定义CORS访问配置](/intl.zh-CN/实例管理/ES集群配置/配置YML文件/自定义CORS访问配置.md)
        -   `http.cors.enabled`
        -   `http.cors.allow-origin`
        -   `http.cors.max-age`
        -   `http.cors.allow-methods`
        -   `http.cors.allow-headers`
        -   `http.cors.allow-credentials`
    -   [配置reindex白名单](/intl.zh-CN/实例管理/ES集群配置/配置YML文件/配置reindex白名单.md)

`reindex.remote.whitelist`

    -   [自定义Auditlog配置](/intl.zh-CN/实例管理/ES集群配置/配置YML文件/自定义Auditlog配置.md)
        -   `xpack.security.audit.enabled`
        -   `xpack.security.audit.index.bulk_size`
        -   `xpack.security.audit.index.flush_interval`
        -   `xpack.security.audit.index.rollover`
        -   `xpack.security.audit.index.events.include`
        -   `xpack.security.audit.index.events.exclude`
        -   `xpack.security.audit.index.events.emit_request_body`
    -   [自定义queue大小](/intl.zh-CN/实例管理/ES集群配置/配置YML文件/自定义queue大小.md)
        -   `thread_pool.bulk.queue_size`（适用于Elasticsearch 5.x版本）
        -   `thread_pool.write.queue_size`（适用于Elasticsearch 6.x及7.x版本）
        -   `thread_pool.search.queue_size`
    -   自定义SQL插件配置

`xpack.sql.enabled`

默认情况下Elasticsearch实例会启用X-Pack自带的SQL插件，如需上传自定义的SQL插件，请将`xpack.sql.enabled`设置为`false`。 |

    **警告：** 修改**YML文件配置**后，需要重启Elasticsearch实例才能生效。此处的重启为滚动重启，如果您集群中的索引有副本，重启后不会对您的业务造成影响，但建议在业务低峰期操作。如果您集群中的索引没有副本，重启会影响您的业务，请确认后再进行下一步操作。

7.  滑动到页面底部，勾选**该操作会重启实例，请确认后操作**，单击**确定**。

    确定后，Elasticsearch实例会进行重启，重启过程中可在[任务列表](/intl.zh-CN/实例管理/管理实例/查看实例任务进度详情.md)中查看进度。重启成功后即可完成YML文件的配置。


