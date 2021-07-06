# 腾讯云Elasticsearch数据迁移至阿里云

本文介绍通过阿里云Logstash，将索引数据从腾讯云Elasticsearch迁移至阿里云Elasticsearch中。

## 操作流程

1.  [准备工作](#section_4ch_ty8_dgr)

    创建阿里云Elasticsearch实例、Logstash实例和腾讯云Elasticsearch实例、开启阿里云Elasticsearch实例的自动创建索引功能、搭建腾讯云代理服务器。

2.  [步骤一：选取迁移方案](#section_ge6_6rj_riq)

    选择兼容的腾讯云Elasticsearch实例、阿里云Elasticsearch实例和Logstash实例进行迁移。

3.  [步骤二：创建并配置Logstash管道](#section_zwd_y2g_js6)

    通过配置文件管理方式创建并配置Logstash管道，配置时设置input为腾讯云Elasticsearch，output为阿里云Elasticsearch。

    **说明：** 您也可以使用自建Logstash，需要在服务器上安装JDK（1.8及以上版本）以及相应版本的Logstash，然后在Logstash的conf下配置YML文件，并启动服务。

4.  [步骤三：验证结果](#section_ptu_4ee_8vv)

    在Kibana中使用`GET /_cat/indices?v`，查看索引是否迁移成功。


## 准备工作

1.  创建阿里云Elasticsearch实例，并开启自动创建索引功能。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)和[t1605396.md\#](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。

2.  创建阿里云Logstash实例。

    具体操作步骤请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

    由于阿里云Logstash实例部署在专有网络VPC（Virtual Private Cloud）下，但在迁移过程中，Logstash需要连接公网才能与腾讯云Elasticsearch互通，因此需要通过配置NAT网关实现与公网连通，详情请参见[配置NAT公网数据传输](/cn.zh-CN/Logstash/网络与安全/配置NAT公网数据传输.md)。

    **说明：** 对于自建的Logstash，需要购买与阿里云Elasticsearch在同一VPC下的ECS实例（已符合条件的ECS不需要重复购买，需要绑定弹性公网IP）。

3.  创建腾讯云Elasticsearch实例，并搭建腾讯云代理服务器。

    由于腾讯云Elasticsearch不支持通过公网直接访问，因此需要准备cvm服务器搭建nginx代理，详情请参见[通过外网访问腾讯云Elasticsearch集群](https://cloud.tencent.com/developer/article/1152363)。


## 步骤一：选取迁移方案

由于腾讯云Elasticsearch版本与阿里云Elasticsearch版本不一致，因此需要先选择兼容的版本进行迁移，本文支持的版本方案如下（其他方案不保证兼容）：

-   腾讯云Elasticsearch 5.6.4 -\> ECS（Logstash 5.6.x）-\> 阿里云Elasticsearch 5.5.6
-   腾讯云Elasticsearch 6.4.3 -\> ECS（Logstash 6.7.0）-\> 阿里云Elasticsearch 6.7.0
-   腾讯云Elasticsearch 6.4.3 -\> 阿里云Logstash 6.7.0 -\> 阿里云Elasticsearch 6.7.0（本文以此为例）

**说明：** 建议您在大版本内进行数据迁移。关于Logstash版本选取详情，请参见[Support Matrix](https://www.elastic.co/cn/support/matrix#matrix_compatibility)。

## 步骤二：创建并配置Logstash管道

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在顶部菜单栏处，选择地域。

3.  在左侧导航栏，单击**Logstash实例**，然后在**Logstash实例**中单击目标实例ID。

4.  在左侧导航栏，单击**管道管理**。

5.  在**管道列表**区域，单击**创建管道**。

    ![创建管道](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p95025.png)

6.  在**创建管道任务**页面，输入**管道ID**并配置管道。

    本文使用的管道配置如下。

    ```
    input {
         elasticsearch {
           hosts => "http://xxxxxxxxx:9200"
           user  => "elastic"
           index => "*"
           password => "xxxxxx"
           docinfo => true
         }
       }
    output {
          elasticsearch {
            hosts => "http://xxxxxxx.elasticsearch.aliyuncs.com:9200"
            user => "elastic"
            password => "xxxxxx"
            index => "%{[@metadata][_index]}"
            document_type => "%{[@metadata][_type]}"
            document_id => "%{[@metadata][_id]}"
      }
    }
    ```

    |参数|说明|
    |--|--|
    |`hosts`|Elasticsearch服务的访问地址。`input`中为`http://<腾讯云代理服务的公网地址>:<端口>`；`output`中为`http://<阿里云Elasticsearch实例ID>.elasticsearch.aliyuncs.com:9200`。|
    |`user`|访问Elasticsearch服务的用户名，默认为elastic。|
    |`password`|对应用户的密码。对于阿里云Elasticsearch，elastic用户的密码在创建实例时设定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |`index`|`input`中为待迁移的索引名，设置为`*`表示同步全部索引；`output`中为迁移后的索引名，设置为`%{[@metadata][_index]}`表示匹配元数据中的index，即迁移后的索引名与源索引名相同。|
    |`document_type`|迁移后索引的类型。设置为`%{[@metadata][_type]}`表示匹配元数据中的type，即迁移后索引的类型与源索引的类型相同。|
    |`document_id`|迁移后文档的ID。设置为`%{[@metadata][_id]}`表示匹配元数据中的id，即迁移后文档的ID与源文档的ID相同。|
    |`docinfo`|设置为`true`，Logstash将会提取Elasticsearch文档的元信息，例如index、type和id。|

    更多Config配置详情请参见[Logstash配置文件说明](/cn.zh-CN/Logstash/管道任务管理/Logstash配置文件说明.md)。

    以上配置将源Elasticsearch集群的所有索引同步到目标集群中，您也可以设置只同步指定的索引，input配置如下。

    ```
    input {
         elasticsearch {
           hosts => "http://xxxxxxxx:9200"
           user  => "elastic"
           index => "alicloudtest"
           password => "xxxxxxx"
           query => '{ "query":{ "query_string": { "query": "*" } } }'
           docinfo => true
         }
    ```

    Elasticsearch input插件可以根据配置的查询语句，从Elasticsearch集群读取文档数据，适用于批量导入测试日志等操作。默认读取完数据后，同步动作会自动关闭。如果您想实现定时触发数据同步，可以通过cron语法配合`schedule`属性实现，详情请参见[Logstash官网Scheduling介绍](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-elasticsearch.html#_scheduling)。

    ```
    schedule => "* * * * *"
    ```

    以上示例会每分钟触发一次数据同步。如果没有指定`schedule`参数，那么同步仅执行一次。

7.  单击**下一步**，配置管道参数。

    ![管道参数配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p67293.png)

    |参数|说明|
    |--|--|
    |**管道工作线程**|并行执行管道的Filter和Output的工作线程数量。当事件出现积压或CPU未饱和时，请考虑增大线程数，更好地使用CPU处理能力。默认值：实例的CPU核数。|
    |**管道批大小**|单个工作线程在尝试执行Filter和Output前，可以从Input收集的最大事件数目。较大的管道批大小可能会带来较大的内存开销。您可以设置LS\_HEAP\_SIZE变量，来增大JVM堆大小，从而有效使用该值。默认值：125。|
    |**管道批延迟**|创建管道事件批时，将过小的批分派给管道工作线程之前，要等候每个事件的时长，单位为毫秒。默认值：50ms。|
    |**队列类型**|用于事件缓冲的内部排队模型。可选值：     -   **MEMORY**：默认值。基于内存的传统队列。
    -   **PERSISTED**：基于磁盘的ACKed队列（持久队列）。 |
    |**队列最大字节数**|请确保该值小于您的磁盘总容量。默认值：1024 MB。|
    |**队列检查点写入数**|启用持久性队列时，在强制执行检查点之前已写入事件的最大数目。设置为0，表示无限制。默认值：1024。|

    **警告：** 配置完成后，需要**保存并部署**才能生效。**保存并部署**操作会触发实例重启，请在不影响业务的前提下，继续执行以下步骤。

8.  单击**保存**或者**保存并部署**。

    -   **保存**：将管道信息保存在Logstash里并触发实例变更，配置不会生效。保存后，系统会返回**管道管理**页面。可在**管道列表**区域，单击**操作**列下的**立即部署**，触发实例重启，使配置生效。
    -   **保存并部署**：保存并且部署后，会触发实例重启，使配置生效。

## 步骤三：验证结果

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中执行`GET /_cat/indices?v`命令，查询索引是否迁移成功。


