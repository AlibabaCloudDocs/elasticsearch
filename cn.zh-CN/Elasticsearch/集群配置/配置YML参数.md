---
keyword: [es yml配置, 自动创建索引, 删除索引指定名称, Auditlog索引, 开启Watcher]
---

# 配置YML参数

通过配置阿里云Elasticsearch实例的YML参数，您可以设置允许自动创建索引、删除索引指定名称、配置Auditlog索引、开启Watcher以及其他配置。本文介绍如何配置YML参数，以及CORS访问、reindex白名单、Auditlog索引和queue大小的配置。

## 注意事项

因阿里云Elasticsearch网络架构调整，2020年10月起创建的实例暂不支持Watcher报警和LDAP认证功能，且不支持与2020年10月前创建的实例进行跨集群Reindex、跨集群搜索、跨集群复制等相关操作。即10月前创建的集群，仅支持与10月前创建的集群进行这些操作；10月后创建的集群仅支持与10月后创建的集群进行这些操作。因网络调整带来的影响，待后期功能上线将会解决，请耐心等待。

## 修改配置

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**ES集群配置**。

5.  在**ES集群配置**页面，单击**YML文件配置**右侧的**修改配置**。

6.  在**YML文件配置**页面，按照以下说明进行配置。

    ![ES YML参数配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6065240061/p40138.png)

    |参数|说明|
    |--|--|
    |**自动创建索引**|当Elasticsearch实例接收到新文档后，如果没有对应索引，是否允许系统自动新建索引。自动创建的索引可能不符合您的预期，不建议开启。 对应的YML文件的配置项为action.auto\_create\_index，默认为false。 |
    |**删除索引指定名称**|在删除索引时是否需要明确指定索引名称。如果选择**删除或关闭时索引名称支持通配符**，则可以使用通配符进行批量删除索引。索引删除后不可恢复，请谨慎使用此配置。 对应的YML文件的配置项为action.destructive\_requires\_name，默认为true。 |
    |**Auditlog索引**|开启后，系统会记录Elasticsearch实例对应的增、删、改、查等操作产生的审计日志，该日志信息会占用您的磁盘空间，同时也会影响性能，不建议开启，请谨慎使用此配置。 更多参数说明，请参见[配置Auditlog](#section_51s_io4_0lq)。**说明：** Elasticsearch 7.0及以上版本暂不支持配置该参数。

对应的YML文件的配置项为xpack.security.audit.enabled，默认为false。 |
    |**开启Watcher**|开启后，可使用X-Pack的Watcher功能。请注意定时清理.watcher-history\*索引，避免占用大量磁盘空间。 对应的YML文件的配置项为xpack.watcher.enabled，默认为false。 |
    |**其他Configure配置**|支持的部分配置项如下（以下配置项，如果没有标识具体适用于哪个Elasticsearch版本，默认兼容Elasticsearch 5.x、6.x和7.x版本）：     -   [配置CORS访问](#section_qq3_vkg_2fb)
        -   http.cors.enabled
        -   http.cors.allow-origin
        -   http.cors.max-age
        -   http.cors.allow-methods
        -   http.cors.allow-headers
        -   http.cors.allow-credentials
    -   [配置reindex白名单](#section_wb5_fwy_qld)

reindex.remote.whitelist

    -   [配置Auditlog](#section_51s_io4_0lq)

**说明：** Elasticsearch 7.0及以上版本不支持配置Auditlog。

        -   xpack.security.audit.enabled
        -   xpack.security.audit.index.bulk\_size
        -   xpack.security.audit.index.flush\_interval
        -   xpack.security.audit.index.rollover
        -   xpack.security.audit.index.events.include
        -   xpack.security.audit.index.events.exclude
        -   xpack.security.audit.index.events.emit\_request\_body
    -   [配置queue大小](#section_s6x_nx8_gyy)
        -   thread\_pool.bulk.queue\_size（适用于Elasticsearch 5.x版本）
        -   thread\_pool.write.queue\_size（适用于Elasticsearch 6.x及7.x版本）
        -   thread\_pool.search.queue\_size
    -   自定义SQL插件配置

xpack.sql.enabled

默认情况下Elasticsearch实例会启用X-Pack自带的SQL插件，如需上传自定义的SQL插件，请将xpack.sql.enabled设置为false。 |

    **警告：** 修改**YML文件配置**后，需要重启Elasticsearch实例才能生效。此处的重启为滚动重启，如果集群中的索引有副本，重启不会影响您的业务，但建议在业务低峰期操作。如果没有副本，重启会影响您的业务，请确认后再进行下一步操作。

7.  滑动到页面底部，勾选**该操作会重启实例，请确认后操作**，单击**确定**。

    确定后，Elasticsearch实例会重启。重启过程中，可在[任务列表](/cn.zh-CN/Elasticsearch/实例管理/查看实例任务进度详情.md)查看进度。重启成功后，即可完成YML文件的配置。


## 配置CORS访问

通过配置跨域资源共享CORS（Cross-origin resource sharing）访问，设置是否允许其他域资源下的浏览器向阿里云Elasticsearch发送请求。您可以在[YML文件配置](#section_8wn_rid_he1)中，配置CORS访问，支持配置的参数如下。

|参数|默认值|说明|
|--|---|--|
|http.cors.enabled|false|设置是否启用跨域资源访问（Elasticsearch是否允许其他域资源下的浏览器向其发送请求）：-   true：启用。Elasticsearch会处理OPTIONS CORS请求。如果发送请求中的域信息已在http.cors.allow-origin中声明，那么Elasticsearch会在头信息中附加Access-Control-Allow-Origin，以响应跨域请求。
-   false：不启用。Elasticsearch会忽略请求头中的域信息，将不会使用Access-Control-Allow-Origin信息头应答。如果客户端不支持发送附加域信息头的preflight请求，或者不校验从服务端返回的报文的头信息中的Access-Control-Allow-Origin信息，那么跨域安全访问将受到影响。如果关闭CORS支持，则客户端只能尝试通过发送OPTIONS请求，以了解此响应信息是否存在。 |
|http.cors.allow-origin|“”|域资源配置项，可设置接受来自哪些域名的请求。默认不允许接受跨域请求且无配置。支持正则表达式，例如/https?:\\/\\/localhost\(:\[0-9\]+\)?/，表示可响应符合此正则的请求信息。**说明：** \*是合法配置，表示集群支持来自任意域名的跨域请求，这将会给Elasticsearch实例带来安全风险，不建议使用。 |
|http.cors.max-age|1728000（20天）|浏览器可发送OPTIONS请求以获取CORS配置信息，此配置项可设置获取的信息在浏览器中的缓存时间，单位为秒。|
|http.cors.allow-methods|OPTIONS, HEAD, GET, POST, PUT, DELETE|请求方法配置项。|
|http.cors.allow-headers|X-Requested-With, Content-Type, Content-Length|请求头信息配置项。|
|http.cors.allow-credentials|false|凭证信息配置项目，设置是否允许响应头中返回Access-Control-Allow-Credentials信息：-   true：允许
-   false：不允许 |

## 配置reindex白名单

通过当前集群调用reindex API，从远程集群迁移索引数据前，需要先配置reindex白名单。您可以在[YML文件配置](#section_8wn_rid_he1)中，配置reindex白名单，支持配置的参数如下。

|参数|默认值|说明|
|--|---|--|
|reindex.remote.whitelist|\[\]|设置远程Elasticsearch集群的访问地址，将其添加到当前集群的远程访问白名单中。白名单支持host和port的组合，并使用逗号分隔多个主机配置（例如otherhost:9200,another:9200,127.0.10.\*\*:9200,localhost:\*\*），不识别协议信息。 |

配置reindex白名单时，如果远程Elasticsearch集群为单可用区的阿里云Elasticsearch实例，请使用<阿里云Elasticsearch实例的域名\>:9200；如果为多可用区实例，请使用实例中所有数据节点的IP地址与端口的组合。具体示例如下：

-   单可用区

    ![单可用区配置示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7202659951/p135190.png)

    ```
    reindex.remote.whitelist: ["es-cn-09k1rgid9000g****.elasticsearch.aliyuncs.com:9200"]
    ```

-   多可用区

    ![多可用区reindex白名单配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6746359951/p135207.png)

    ```
    reindex.remote.whitelist: ["10.0.xx.xx:9200","10.0.xx.xx:9200","10.0.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200"]
    ```

    **说明：** reindex白名单配置完成后，即可调用reindex API重建索引。具体操作，请参见[通过reindex迁移数据](/cn.zh-CN/最佳实践/Elasticsearch迁移/阿里云ES间数据迁移/通过reindex迁移数据.md)。


## 配置Auditlog

您可以在[YML文件配置](#section_8wn_rid_he1)中，开启Elasticsearch的Auditlog索引，查看相关日志文件。开启后，您可以自定义Auditlog配置，示例如下。

```
xpack.security.audit.index.bulk_size: 5000
xpack.security.audit.index.events.emit_request_body: false
xpack.security.audit.index.events.exclude: run_as_denied,anonymous_access_denied,realm_authentication_failed,access_denied,connection_denied
xpack.security.audit.index.events.include: authentication_failed,access_granted,tampered_request,connection_granted,run_as_granted
xpack.security.audit.index.flush_interval: 180s
xpack.security.audit.index.rollover: hourly
xpack.security.audit.index.settings.index.number_of_replicas: 1
xpack.security.audit.index.settings.index.number_of_shards: 10
```

|配置|默认设置|说明|
|--|----|--|
|xpack.security.audit.index.bulk\_size|1000|当您将多个审计事件分批写入到一个Auditlog索引中时，可通过该参数，设置写入事件的数量。|
|xpack.security.audit.index.flush\_interval|1s|控制缓冲事件刷新到索引的频率。|
|xpack.security.audit.index.rollover|daily|控制滚动构建到新索引的频率，可以设置为hourly、daily、weekly或monthly。|
|xpack.security.audit.index.events.include|access\_denied, access\_granted, anonymous\_access\_denied, authentication\_failed, connection\_denied, tampered\_request, run\_as\_denied, run\_as\_granted|控制何种Auditlog事件可以被写入到索引中。完整事件类型列表，请参见[Accesslog事件类型](https://www.elastic.co/guide/en/x-pack/5.5/auditing.html#audit-event-types)。|
|xpack.security.audit.index.events.exclude|null（默认不处理任何事件）|构建索引过程中，排除的Auditlog事件。|
|xpack.security.audit.index.events.emit\_request\_body|false|当触发明确的事件类型时（例如authentication\_failed），是否忽略或包含以REST发送的请求体。|

**说明：**

-   当Auditlog中包含Requestbody信息时，可能会在日志文件中暴露敏感信息。
-   当开启Auditlog后，Auditlog日志文件将输出到Elasticsearch实例中，并使用.security\_audit\_log-\*开头的索引名称，该索引将占用实例的存储空间。由于Elasticsearch不支持自动过期清除策略，因此需要手动清除旧的Auditlog索引。

您也可以通过xpack.security.audit.index.settings，配置存储Auditlog的索引。以下配置构建Auditlog索引的分片和副本均为1。

```
xpack.security.audit.index.settings:
  index:
    number_of_shards: 1
    number_of_replicas: 1
```

**说明：** 如果您希望通过传入配置参数生成Auditlog索引，请在开启Auditlog索引（设置xpack.security.audit.enabled为true）的同时传入此配置。否则，Auditlog索引将使用默认的number\_of\_shards: 5、number\_of\_replicas: 1配置。

详细信息，请参见[Auditing Security Settings](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/auditing-settings.html#auditing-settings)。

## 配置queue大小

通过自定义queue大小，调整文档写入和搜索的队列大小。您可以在[YML文件配置](#section_8wn_rid_he1)中，配置queue大小。以下示例配置文档写入和搜索queue大小为500和1200，实际业务中请根据具体情况自行调整。

```
thread_pool.bulk.queue_size: 500
thread_pool.write.queue_size: 500
thread_pool.search.queue_size: 1200
```

|参数|默认值|说明|
|--|---|--|
|thread\_pool.bulk.queue\_size|200|文档写入队列大小，适用于阿里云Elasticsearch 5.x版本。|
|thread\_pool.write.queue\_size|200|文档写入队列大小，适用于阿里云Elasticsearch 6.x及7.x版本。|
|thread\_pool.search.queue\_size|1000|文档搜索队列大小。|

