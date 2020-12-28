---
keyword: [logstash管道配置调试, logstash管道配置调试日志]
---

# 使用Logstash管道配置调试功能

当Logstash管道配置出现错误时，可能导致输出数据结果不符合预期，需要反复去目标端确认数据格式，再返回控制台修改配置，这样会耗费大量的时间和人力。对于这种情况，您可以通过阿里云Logstash提供的管道配置调试功能，在管道配置完成后，直接在控制台上查看管道配置的输出结果，降低调试成本。本文介绍具体的实现方法。

已安装logstash-output-file\_extend插件。如果还未安装，请先安装该插件，安装方法请参见[安装Logstash插件](/intl.zh-CN/Logstash/插件配置/安装Logstash插件.md)。

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Logstash实例**。

3.  在顶部菜单栏处，选择地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**管道管理**。

5.  单击**创建管道**。

6.  配置并启动管道。

    1.  在**Config配置**中，填写**管道ID**和**Config配置**。

        ![config配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7629919951/p127138.png)

        |参数|说明|
        |--|--|
        |**管道ID**|自定义输入。输入后，管道ID会自动映射到file\_extend的path路径下。|
        |**Config配置**|**Config配置**由三部分组成：         -   input：指定待读取的数据源，支持Logstash自带的[input plugins](https://www.elastic.co/guide/en/logstash/6.7/input-plugins.html)（除过file插件）。
        -   filter：进一步加工处理数据源采集到的数据，支持丰富的[Filter plugins](https://www.elastic.co/guide/en/logstash/6.7/filter-plugins.html)。
        -   output：将过滤后的数据发送到特定的目的端。阿里云Logstash不仅支持开源的 [Logstash output plugins](https://www.elastic.co/guide/en/logstash/6.7/output-plugins.html)，还可通过配置特有的file\_extend插件，开启调试日志功能，即可在管道部署完成后直接查看输出结果，并进行验证与调试。 |

        config配置示例如下。

        ```
        input {
             elasticsearch {
               hosts => "http://es-cn-0pp1jxv000****.elasticsearch.aliyuncs.com:9200"
               user  => "elastic"
               index => "twitter"
               password => "<your_password>"
               docinfo => true
               schedule => "* * * * *"
             }
        
        }
        filter {
        
        }
        output {
            elasticsearch {
            hosts => ["http://es-cn-000000000i****.elasticsearch.aliyuncs.com:9200"]
            user => "elastic"
            password => "<your_password>"
            index => "%{[@metadata][_index]}"
            document_id => "%{[@metadata][_id]}"
        
          }
           file_extend {
             path => "/ssd/1/ls-cn-v0h1kzca****/logstash/logs/debug/test"
           }
        
        }
        ```

        **说明：**

        -   output中的file\_extend配置默认为注释状态，如果需要使用调试功能，请先删除注释。
        -   file\_extend中的path参数默认为系统指定路径，请勿修改。您也可以单击**开启配置调试**获取path路径。
        -   path中的\{pipelineid\}将自动映射为管道ID，请勿修改为其他名称，否则无法获取调试日志。
    2.  单击**下一步**，配置管道参数。

        管道配置参数的详细信息，请参见[通过配置文件管理管道](/intl.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)。

    3.  保存并部署管道。

        -   **保存**：将管道信息保存在Logstash里并触发实例变更，配置不会生效。保存后，系统会返回**管道管理**页面。可在**管道列表**区域，单击**操作**列下的**立即部署**，触发实例重启，使配置生效。
        -   **保存并部署**：保存并且部署后，会触发实例重启，使配置生效。
    4.  在创建成功提示框中，单击**确认**。

7.  查看调试日志。

    1.  等待实例重启完成后，在**管道列表**中，单击目标管道右侧**操作**列下的**查看调试日志**。

    2.  在**日志查询**页面的**调试日志**页签中，获取管道处理后的输出数据。

        对于多个管道，您可在搜索框中输入pipelineId: <管道ID\>过滤对应的日志。

        ![查看调试日志](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7629919951/p127155.png)


