---
keyword: beats采集日志
---

# 通过Filebeat采集Apache日志数据

当您需要查看并分析Apache日志数据时，可以使用Filebeat采集日志数据，并通过阿里云Logstash过滤采集后的日志数据，最终传输到Elasticsearch中进行分析。本文介绍如何通过Filebeat采集Apache日志数据。

您已完成以下操作：

-   创建阿里云Elasticsearch实例和Logstash实例，两者版本相同，并且在同一专有网络VPC（Virtual Private Cloud）下。

    具体操作，请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)和[创建阿里云Logstash实例](/cn.zh-CN/Logstash/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

-   开启阿里云Elasticsearch实例的**自动创建索引**功能。

    处于安全考虑，阿里云Elasticsearch默认不允许**自动创建索引**。但是Beats目前依赖该功能，因此如果**采集器Output**选择为**Elasticsearch**，需要开启自动创建索引功能。具体操作，请参见[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。

-   创建阿里云ECS实例，并且该ECS实例与阿里云Elasticsearch实例和Logstash实例处于同一VPC下。

    具体操作，请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

    **说明：** Beats目前仅支持Aliyun Linux、RedHat和CentOS这三种操作系统。

-   在ECS实例上搭建Httpd服务。

    为了便于通过可视化工具分析展示日志，建议在httpd.conf中将Apache日志格式定义为JSON格式，详情请参见[Apache官方说明](https://cwiki.apache.org/confluence/display/httpd/Apache)。本文的测试环境配置如下。

    ```
    LogFormat "{\"@timestamp\":\"%{%Y-%m-%dT%H:%M:%S%z}t\",\"client_ip\":\"%{X-Forwa rded-For}i\",\"direct_ip\": \"%a\",\"request_time\":%T,\"status\":%>s,\"url\":\"%U%q\",\"method\":\"%m\",\"http_host\":\"%{Host}i\",\"server_ip\":\"%A\",\"http_referer\":\"%{Referer}i\",\"http_user_agent\":\"%{User-agent}i\",\"body_bytes_sent\":\"%B\",\"total_bytes_sent\":\"%O\"}"  access_log_json
    # 注销原有CustomLog修改为CustomLog "logs/access_log" access_log_json
    ```

-   在目标ECS实例上安装云助手和Docker服务。

    具体操作，请参见[安装云助手客户端](/cn.zh-CN/运维与监控/云助手/配置云助手客户端/安装云助手客户端.md)和[部署并使用Docker（Alibaba Cloud Linux 2）](/cn.zh-CN/建站教程/搭建应用/部署并使用Docker/部署并使用Docker（Alibaba Cloud Linux 2）.md)。


## 操作步骤

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在**新建采集器**区域，单击**Filebeat**。

3.  安装并配置采集器。

    具体操作，请参见[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)和[采集器YML配置](/cn.zh-CN/Beats/采集器YML配置.md)，本文使用的配置如下。

    ![filebeat配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6012659951/p82408.png)

    **说明：**

    -   采集器Output需要指定目标阿里云Logstash的实例ID，在YML配置中不需要重新指定Output。
    -   Filebeat文件目录需要填写数据源所在的目录，同时需要在YML配置中开启log数据采集，并配置log路径。
4.  单击**下一步**。

5.  在**采集器安装**配置向导中，选择安装采集器的ECS实例。

    ![选择采集器安装的实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82419.png)

    **说明：** 所选择的ECS实例，需要满足上文的前提条件。

6.  启动采集器并查看采集器安装情况。

    1.  单击**启动**。

        启动成功后，系统弹出**启动成功**对话框。

    2.  单击**前往采集中心查看**，返回**Beats数据采集中心**页面，在**采集器管理**区域中，查看启动成功的Filebeat采集器。

    3.  等待**采集器状态**变为**已生效1/1**后，单击右侧**操作**栏下的**查看运行实例**。

    4.  在**查看运行实例**页面，查看**采集器安装情况**，当显示为**心跳正常**时，说明采集器安装成功。

        ![查看采集器安装状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82426.png)

7.  配置Logstash管道。

    1.  在左侧导航栏，单击**Logstash实例**。

    2.  单击目标Logstash实例右侧**操作**列下的**管道管理**。

    3.  在**管道管理**页面，单击**创建管道**。

    4.  配置管道。

        参考如下示例配置管道，详细配置方法请参见[通过配置文件管理管道](/cn.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)。

        ```
        input {
          beats {
              port => 8000
            }
        }
        filter {
          json {
                source => "message"
                remove_field => "@version"
                remove_field => "prospector"
                remove_field => "beat"
                remove_field => "source"
                remove_field => "input"
                remove_field => "offset"
                remove_field => "fields"
                remove_field => "host"
                remove_field => "message"
              }
        
        }
        output {
          elasticsearch {
            hosts => ["http://es-cn-mp91cbxsm00******.elasticsearch.aliyuncs.com:9200"]
            user => "elastic"
            password => "<your_password>"
            index => "<your_index>"
          }
        }
        ```

        |参数|说明|
        |--|--|
        |`input`|接收Beats采集的数据。|
        |`filter`|过滤采集的数据。通过`json`插件进行message数据解码，使用`remove_field`删除指定字段。 **说明：** 本文中的`filter`配置只适用于当前测试场景，不适用于所有的业务场景。请根据自身业务场景修改`filter`配置，关于`filter`支持的插件及每个插件的使用说明，请参见[filter plugin](https://www.elastic.co/guide/en/logstash/6.7/filter-plugins.html#filter-plugins)。 |
        |`output`|将数据传输至阿里云ES实例中。 `<your_password>`需要替换为您阿里云ES实例的访问密码；`<your_index>`需要替换为您定义的索引名称。 |


## 验证结果

1.  登录目标阿里云ES实例的Kibana控制台。

    登录控制台的具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中执行如下命令查看采集成功的数据。

    ```
    GET <your_index>/_search
    ```

    **说明：** `<your_index>`需要替换为在配置阿里云Logstash实例的管道时，output中所定义的索引名称。

4.  在左侧导航栏，单击**Discover**，选择一段时间，查看采集的数据详情。

    ![查看采集数据详情](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6012659951/p82431.png)

    **说明：** 在查询前，请确保您已经创建了`<your_index>`的索引模式。否则需要在Kibana控制台中，单击**Management**，再单击**Kibana**区域中的**Index Patterns** \> **Create index pattern**，按照提示创建索引模式。


