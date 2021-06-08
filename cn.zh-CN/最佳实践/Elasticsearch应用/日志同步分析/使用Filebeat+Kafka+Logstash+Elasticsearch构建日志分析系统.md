# 使用Filebeat+Kafka+Logstash+Elasticsearch构建日志分析系统

随着时间的积累，日志数据会越来越多，当您需要查看并分析庞杂的日志数据时，可通过Filebeat+Kafka+Logstash+Elasticsearch采集日志数据到阿里云Elasticsearch（简称ES）中，并通过Kibana进行可视化展示与分析。本文介绍具体的实现方法。

Kafka是一种分布式、高吞吐、可扩展的消息队列服务，广泛用于日志收集、监控数据聚合、流式数据处理、在线和离线分析等大数据领域，已成为大数据生态中不可或缺的部分。本文使用阿里云消息队列Kafka版，详情请参见[什么是消息队列Kafka版](/cn.zh-CN/产品简介/什么是消息队列Kafka版？.md)。

在实际应用场景中，为了满足大数据实时检索的需求，您可以使用Filebeat采集日志数据，将Kafka作为Filebeat的输出端。Kafka实时接收到Filebeat采集的数据后，以Logstash作为输出端输出。输出到Logstash中的数据在格式或内容上可能不能满足您的需求，此时可以通过Logstash的filter插件过滤数据。最后将满足需求的数据输出到ES中进行分布式检索，并通过Kibana进行数据分析与展示。简单流程如下。

![流程图](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5902659951/p111354.png)

## 操作流程

1.  [准备工作](#section_awk_u3v_vu4)

    完成环境准备，包括创建实例、安装Filebeat等。

2.  [步骤一：配置Filebeat](#section_lp3_0ku_60g)

    配置Filebeat的input为系统日志，output为Kafka，将日志数据采集到Kafka的指定Topic中。

3.  [步骤二：配置Logstash管道](#section_sgb_wlr_5fc)

    配置Logstash管道的input为Kafka，output为阿里云ES，使用Logstash消费Topic中的数据并传输到阿里云ES中。

4.  [步骤三：查看日志消费状态](#section_nkj_q99_zlq)

    在消息队列Kafka中查看日志数据的消费的状态，验证日志数据是否采集成功。

5.  [步骤四：通过Kibana过滤日志数据](#section_w9z_gib_3s1)

    在Kibana控制台的Discover页面，通过Filter过滤出Kafka相关的日志。


## 准备工作

1.  创建阿里云ES实例，并开启实例的自动创建索引功能。

    具体操作步骤请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)，本文以6.7版本为例。

2.  创建阿里云Logstash实例。要求该实例与阿里云ES实例的版本相同，并且在同一专有网络VPC（Virtual Private Cloud）下。

    具体操作步骤请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

3.  购买并部署阿里云消息队列Kafka版实例、创建Topic和Consumer Group。

    本文使用VPC实例，并要求该实例与阿里云ES实例在同一VPC下，具体操作步骤请参见[VPC接入](/cn.zh-CN/快速入门/步骤二：购买和部署实例/VPC接入.md)。

    创建Topic和Consumer Group的具体步骤请参见[步骤三：创建资源](/cn.zh-CN/快速入门/步骤三：创建资源.md)。

4.  创建阿里云ECS实例，并且该ECS实例与阿里云ES实例和Logstash实例处于同一VPC下。

    具体操作步骤请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

    **说明：** 该ECS实例用来安装Filebeat，由于Filebeat目前仅支持Aliyun Linux、RedHat和CentOS这三种操作系统，因此在创建时请选择其中一种操作系统。

5.  在ECS实例上安装Filebeat。

    具体操作步骤请参见[Install Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html)。本文使用[6.8.5版本](https://www.elastic.co/cn/downloads/past-releases/filebeat-6-8-5)。


## 步骤一：配置Filebeat

1.  连接安装了Filebeat的ECS服务器。

    具体操作步骤请参见[连接实例](/cn.zh-CN/实例/连接实例/连接方式概述.md)。

2.  进入Filebeat安装目录，执行以下命令，创建并配置filebeat.kafka.yml文件。

    ```
    cd filebeat-6.8.5-linux-x86_64
    vi filebeat.kafka.yml
    ```

    filebeat.kafka.yml配置如下。

    ```
    filebeat.prospectors:
      - type: log
        enabled: true
        paths:
            - /var/log/*.log
    
    output.kafka:
        hosts: ["172.16.**.**:9092","172.16.**.**:9092","172.16.**.**:9092"]
        topic: estest
        version: 0.10.2
    ```

    |参数|说明|
    |--|--|
    |`type`|输入类型。设置为log，表示输入源为日志。|
    |`enabled`|设置配置是否生效。true表示生效，false表示不生效。|
    |`paths`|需要监控的日志文件的路径。多个日志可在当前路径下另起一行写入日志文件路径。|
    |`hosts`|消息队列Kafka实例的接入点，可在实例详情页面获取，详情请参见[查看接入点](/cn.zh-CN/用户指南/实例/查看接入点.md)。由于本文使用的是VPC实例，因此使用默认接入点。|
    |`topic`|日志输出到消息队列Kafka的Topic，请指定为您已创建的Topic。|
    |`version`|Kafka的版本，可在消息队列Kafka的实例详情页面获取。 **说明：** 不配置此参数会报错。 |

3.  启动Filebeat。

    ```
    ./filebeat -e -c filebeat.kafka.yml
    ```


## 步骤二：配置Logstash管道

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在左侧导航栏，单击**Logstash实例**，然后在**Logstash实例**中单击目标实例ID。

4.  在左侧导航栏，单击**管道管理**。

5.  在**管道列表**区域，单击**创建管道**。

    ![创建管道](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p95025.png)

6.  在**创建管道任务**页面，输入**管道ID**并配置管道。

    本文使用的管道配置如下。

    ```
    input {
      kafka {
        bootstrap_servers => ["172.**.**.92:9092,172.16.**.**:9092,172.16.**.**:9092"]
        group_id => "es-test"
        topics => ["estest"] 
        codec => json
     }
    }
    filter {
    
    }
    output {
      elasticsearch {
        hosts => "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
        user =>"elastic"
        password =>"<your_password>"
        index => "kafka‐%{+YYYY.MM.dd}"
     }
    }
    ```

    |参数|说明|
    |--|--|
    |`bootstrap_servers`|消息队列Kafka实例的接入点，可在实例详情页面获取，详情请参见[查看接入点](/cn.zh-CN/用户指南/实例/查看接入点.md)。由于本文使用的是VPC实例，因此使用默认接入点。|
    |`group_id`|指定为您已创建的Consumer Group的名称。|
    |`topics`|指定为您已创建的Topic的名称，需要与Filebeat中配置的Topic名称保持一致。|
    |`codec`|设置为json，表示解析JSON格式的字段，便于在Kibana中分析。|

    |参数|说明|
    |--|--|
    |`hosts`|阿里云ES的访问地址，取值为`http://<阿里云ES实例的内网地址>:9200`。 **说明：** 您可在阿里云ES实例的基本信息页面获取其内网地址，详情请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。 |
    |`user`|访问阿里云ES的用户名，默认为elastic。您也可以使用自建用户，详情请参见[创建用户](/cn.zh-CN/访问控制/Kibana角色管理/创建用户.md)。|
    |`password`|访问阿里云ES的密码，在创建实例时设置。如果忘记密码，可进行重置，重置密码的注意事项及操作步骤请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |`index`|索引名称。设置为`kafka‐%{+YYYY.MM.dd}`表示索引名称以kafka为前缀，以日期为后缀，例如`kafka-2020.05.27`。|

    更多Config配置详情请参见[Logstash配置文件说明](/cn.zh-CN/Logstash/管道任务管理/Logstash配置文件说明.md)。

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

## 步骤三：查看日志消费状态

1.  进入[消息队列Kafka控制台](https://kafka.console.aliyun.com/)。

2.  在左侧导航栏，单击**Consumer Group管理**。

3.  在**Consumer Group管理**页面，单击目标消息队列Kafka实例。

4.  单击对应Consumer Group右侧**操作**列下的**消费状态**。

    ![查看消费状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5902659951/p111302.png)

5.  在**消费状态**对话框中，单击对应Topic右侧**操作**列下的**详情**，查看详细消费状态。

    正常情况下，返回结果如下。

    ![查看消费详情](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5902659951/p111300.png)


## 步骤四：通过Kibana过滤日志数据

1.  登录目标阿里云ES实例的Kibana控制台。

    具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  创建一个索引模式。

    1.  在左侧导航栏，单击**Management**。

    2.  在Kibana区域，单击**Index Patterns**。

    3.  单击**Create index pattern**。

    4.  输入**Index pattern**（本文使用kafka-\*），单击**Next step**。

        ![创建索引模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5902659951/p111286.png)

    5.  选择**Time Filter field name**（本文选择**@timestamp**），单击**Create index pattern**。

        ![Time Filter field name](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5902659951/p109738.png)

3.  在左侧导航栏，单击**Discover**。

4.  从页面左侧的下拉列表中，选择您已创建的索引模式（kafka-\*）。

5.  在页面右上角，选择一段时间，查看对应时间段内的Filebeat采集的日志数据。

    ![查看日志数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5902659951/p111291.png)

6.  单击**Add a filter**，在**Add filter**页面中设置过滤条件，查看符合对应过滤条件的日志数据。

    ![过滤日志数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6902659951/p111294.png)


