---
keyword: [自建es数据迁移, 阿里云logstash数据迁移]
---

# 通过阿里云Logstash将自建Elasticsearch数据迁移至阿里云

当您需要将自建Elasticsearch中的数据迁移到阿里云Elasticsearch中时，可以通过阿里云Logstash的管道配置功能实现。本文介绍具体的实现方法。

您已完成以下操作：

-   准备自建Elasticsearch集群。

    建议您使用阿里云ECS搭建自建Elasticsearch集群。具体操作，请参见[安装并运行Elasticsearch](https://www.elastic.co/guide/cn/elasticsearch/guide/current/running-elasticsearch.html)。

    **说明：**

    -   自建Elasticsearch集群所在的ECS的网络类型必须是专有网络，不支持Classiclink方式打通的ECS。
    -   由于阿里云Logstash实例部署在专有网络下，如果自建Elasticsearch与阿里云Logstash在同一专有网络下，可直接配置；如果不在，需要通过配置NAT网关实现与公网的连通，详情请参见[配置NAT公网数据传输](/cn.zh-CN/Logstash实例/网络与安全/配置NAT公网数据传输.md)。
    -   自建Elasticsearch所在的ECS的安全组不能限制Logstash实例的各节点IP（可在实例基本信息页面查看），并且需要开启9200端口。
    -   本文以自建Elasticsearch 5.6.16 -\> 阿里云Logstash 6.7.0 -\> 阿里云Elasticsearch 6.7.0为例，提供的脚本仅适用于该数据迁移方案，其他方案不保证兼容。
-   创建阿里云Logstash实例。

    具体操作，请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash实例/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

-   创建目标阿里云Elasticsearch实例，需要与Logstash实例在同一专有网络下，且版本相同。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   开启Elasticsearch实例的自动创建索引功能。

    具体操作，请参见[开启自动创建索引](/cn.zh-CN/快速入门/步骤二：配置实例（可选）.md)。


## 配置并运行Logstash管道

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**管道管理**。

5.  在**管道列表**区域，单击**创建管道**。

    ![创建管道](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p95025.png)

6.  在**创建管道任务**页面，输入管道ID并配置管道。

    本文使用的管道配置如下。

    ```
    input {
        elasticsearch {
        hosts => ["http://<自建Elasticsearch Master节点的IP地址>:9200"]
        user => "elastic"
        index => "*,-.monitoring*,-.security*,-.kibana*"
        password => "your_password"
        docinfo => true
      }
    }
    filter {
    }
    output {
      elasticsearch {
        hosts => ["http://es-cn-mp91cbxsm000c****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        password => "your_password"
        index => "%{[@metadata][_index]}"
        document_type => "%{[@metadata][_type]}"
        document_id => "%{[@metadata][_id]}"
      }
    }
    ```

    |参数|描述|
    |--|--|
    |hosts|自建或阿里云Elasticsearch服务的访问地址。input中为`http://<自建Elasticsearch Master节点的IP地址>:<端口>`；output中为`http://<阿里云Elasticsearch实例ID>.elasticsearch.aliyuncs.com:9200`。|
    |user|访问自建或阿里云Elasticsearch服务的用户名。 **说明：**

    -   user和password为必选参数。如果自建Elasticsearch未安装X-Pack，可将这两个参数值设置为空。
    -   访问阿里云Elasticsearch实例的用户名默认为elastic（本文以此为例）。如果想使用自建用户，需要为自建用户分配相应的角色和权限，详情请参见[创建角色](/cn.zh-CN/ES访问控制/Kibana角色管理/创建角色.md)和[创建用户](/cn.zh-CN/ES访问控制/Kibana角色管理/创建用户.md)。 |
    |password|访问自建或阿里云Elasticsearch服务的密码。|
    |index|指定同步索引名。input中设置为\*,-.monitoring\*,-.security\*,-.kibana\*，表示同步除过`.`开头的系统索引；output中设置为%\{\[@metadata\]\[\_index\]\}，表示匹配元数据中的index，即阿里云Elasticsearch生成的索引和自建Elasticsearch的索引相同。|
    |docinfo|设置为true，阿里云Elasticsearch会提取自建Elasticsearch文档的元数据信息，例如index、type和id。|
    |document\_type|设置为%\{\[@metadata\]\[\_type\]\}，表示匹配元数据中索引的type，即阿里云Elasticsearch生成的索引的类型和自建Elasticsearch的索引类型相同。|
    |document\_id|设置为%\{\[@metadata\]\[\_id\]\}，表示匹配元数据中文档的id，即阿里云Elasticsearch生成的文档id和自建Elasticsearch的文档id相同。|

    更多Config配置说明，请参见[Logstash配置文件说明](/cn.zh-CN/Logstash实例/管道任务管理/Logstash配置文件说明.md)。

7.  单击**下一步**，配置管道参数。

    ![管道参数配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p67293.png)

    |参数|说明|
    |--|--|
    |**管道工作线程**|并行执行管道的Filter和Output的工作线程数量。当事件出现积压或CPU未饱和时，请考虑增大线程数，更好地使用CPU处理能力。默认值：实例的CPU核数。|
    |**管道批大小**|单个工作线程在尝试执行Filter和Output前，可以从Input收集的最大事件数目。较大的管道批大小可能会带来较大的内存开销。您可以设置LS\_HEAP\_SIZE变量，来增大JVM堆大小，从而有效使用该值。默认值：125。|
    |**管道批延迟**|创建管道事件批时，将过小的批分派给管道工作线程之前，要等候每个事件的时长，单位为毫秒。默认值：50ms。|
    |**队列类型**|用于事件缓冲的内部排队模型。可选值：     -   **MEMORY**：默认值。基于内存的传统队列。
    -   **PERSISTED**：基于磁盘的ACKed队列（持久队列）。 |
    |**队列最大字节数**|请确保该值小于您的磁盘总容量。默认值：1024MB。|
    |**队列检查点写入数**|启用持久性队列时，在强制执行检查点之前已写入事件的最大数目。设置为0，表示无限制。默认值：1024。|

    **警告：** 配置完成后，需要**保存并部署**才能生效。**保存并部署**操作会触发实例重启，请在不影响业务的前提下，继续执行以下步骤。

8.  单击**保存**或者**保存并部署**。

    -   **保存**：将管道信息保存在Logstash里并触发实例变更，配置不会生效。保存后，系统会返回**管道管理**页面。可在**管道列表**区域，单击**操作**列下的**立即部署**，触发实例重启，使配置生效。
    -   **保存并部署**：保存并且部署后，会触发实例重启，使配置生效。

## 验证结果

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/ES实例/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中，执行`GET /_cat/indices?v`命令，查看迁移成功的索引。

    ![迁移成功的索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1302659951/p95323.png)


## 常见问题

Q：自建Elasticsearch所在的ECS与阿里云Logstash不在同一账号下，迁移数据时，如何配置网络互通？

A：由于ECS和Logstash不在同一账号下，那么两者的专有网络必然不同，因此需要配置两个专有网络互通。可通过云企业网来实现。具体操作，请参见[步骤三：加载网络实例]()。

