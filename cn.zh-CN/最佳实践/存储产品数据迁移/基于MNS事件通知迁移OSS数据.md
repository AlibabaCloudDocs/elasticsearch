---
keyword: MNS事件通知迁移OSS数据
---

# 基于MNS事件通知迁移OSS数据

当对象存储服务OSS（Object Storage Service）文件发生变更，触发阿里云消息服务MNS（Message Notification Service）事件通知时，您可以通过阿里云Logstash的logstash-input-oss插件获取OSS变更事件，再通过logstash-output-oss插件将数据同步到OSS Bucket中。本文介绍对应的配置方法。

您已完成以下操作：

-   安装logstash-input-oss和logstash-output-oss插件。

    详情请参见[安装Logstash插件](/cn.zh-CN/Logstash/插件配置/安装Logstash插件.md)。

-   开通阿里云OSS服务并准备数据。

    详情请参见[开通OSS服务](/cn.zh-CN/控制台用户指南/开通OSS服务.md)。

-   开通消息服务MNS，并确保与OSS服务在同一区域。

    详情请参见[开通消息服务MNS并授权]()。

    **说明：** 本示例设置事件通知类型为PostObject、PutObject，即触发Post、Put事件请求，logstash-output-oss插件同步此数据到对端OSS。


## 操作步骤

1.  [步骤一：配置事件通知规则](#section_b2m_3nv_lx2)

2.  [步骤二：配置Logstash管道](#section_osl_yhi_m6t)

3.  [步骤三：查看数据同步结果](#section_00c_cjj_xmm)


## 步骤一：配置事件通知规则

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  在左侧导航栏，单击**Bucket列表**，再单击目标**Bucket名称**。

3.  单击**基础设置** \> **事件通知**。

4.  在**事件通知**区域，单击**设置**。

5.  单击**创建规则**。

6.  在**创建规则**页面，配置事件通知规则。

    本示例配置的事件通知如下。

    ![事件通知配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p74523.png)

    配置参数详情请参见[设置事件通知规则](/cn.zh-CN/控制台用户指南/存储空间管理/基础设置/设置事件通知规则.md)。

    **说明：** **资源描述**目录中不能包含特殊字符，更多事件类型请参见[事件通知](/cn.zh-CN/开发指南/事件通知.md)。

7.  单击**确定**。

8.  查看事件通知规则。

    1.  进入[消息服务MNS控制台](https://mns.console.aliyun.com/#/?regionId=cn-hangzhou)。

    2.  在顶部菜单栏选择地域。

    3.  在左侧导航栏，单击**事件通知**。


## 步骤二：配置Logstash管道

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在顶部菜单栏处，选择地域。

3.  在**实例列表**页面，单击**实例ID/名称**链接，或者单击**操作**栏下的**实例管理**。

4.  单击左侧导航栏的**管道管理**。

5.  在**管道列表**区域，单击**创建管道**。

    ![创建管道](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p95025.png)

6.  在**创建管道任务**页面，进行Config配置。

    本文使用的Config配置如下。

    ```
    input {
      oss {
        endpoint => "oss-cn-hangzhou-internal.aliyuncs.com"
        bucket => "zl-ossoutoss"
        access_key_id => "LTAIaX42ddd******"
        access_key_secret => "zuyBRUUndddddds3e6i******"
        mns_settings => {
           endpoint => "18185036364****.mns.cn-hangzhou-internal.aliyuncs.com"
           queue => "zl-test"
        }
        codec => json {
          charset => "UTF-8"
        }
      }
    }
    
    output {
      oss {
        endpoint => "http://oss-cn-hangzhou-internal.aliyuncs.com"              
        bucket => "zl-log-output-test"                          
        access_key_id => "LTAIaxxxxx******"                 
        access_key_secret => "zuxxxx8hBpXs3e6i******"         
        prefix => "oss/database"                         
        recover => true                                      
        rotation_strategy => "size_and_time"                  
        time_rotate => 1                                     
        size_rotate => 1000
        temporary_directory => "/ssd/1/ls-cn-0pp1cwec****/logstash/data/22"                            
        encoding => "gzip"                                 
        additional_oss_settings => {
          max_connections_to_oss => 1024                      
          secure_connection_enabled => false                  
        }
    
      }
    }
    ```

    **说明：**

    -   MNS Endpoint不能加HTTP前缀，并且是internal域名，否则报错。
    -   通过`time_rotate`及`size_rotate`，设置当临时目录数据保存1分钟或者大小达到1000Bytes时，触发数据上传。
    -   input参数说明请参见[logstash-input-oss插件参数说明](/cn.zh-CN/Logstash/插件配置/logstash-input-oss插件使用说明.md)，output参数说明请参见[logstash-output-oss插件参数说明](/cn.zh-CN/Logstash/插件配置/logstash-output-oss插件使用说明.md)。
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

## 步骤三：查看数据同步结果

1.  进入[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  上传新的文件到MNS监控的目录中（上文配置事件通知规则时定义的**资源描述**）。

    上传文件的方法请参见[上传文件](/cn.zh-CN/快速入门/控制台快速入门/上传文件.md)。

3.  进入目标端OSS Bucket，查看同步成功的数据。

    **说明：** OSS插件不是以文档或目录形式进行数据同步，而是按照原文档中的数据一条一条进行读写。根据rotate规则，logstash-output-oss插件先将数据存储在本地临时文件中，当达到一定的时间或者大小再进行上传，所以不能保证同一个文档的数据保存到相同的文档下，例如可能会出现多个文档的部分数据同步到一个文档中的情况。


