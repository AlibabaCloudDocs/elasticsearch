---
keyword: SkyWalking es
---

# 使用SkyWalking和Elasticsearch实现全链路监控

SkyWalking是分布式的应用性能管理APM（Application Performance Monitoring）工具，也被称为分布式追踪系统。本文介绍使用阿里云Elasticsearch 7.4版本的实例与SkyWalking，实现对实例的全链路监控。

SkyWalking具有以下特性：

-   全自动探针监控，不需要修改应用程序代码。
-   手动探针监控，提供了支持OpenTracing标准的SDK。覆盖范围扩大到OpenTracing-Java支持的组件。

    **说明：** OpenTracing支持的组件请参见[OpenTracing Registry](https://opentracing.io/registry/)。

-   自动监控和手动监控可以同时使用，使用手动监控弥补自动监控不支持的组件，甚至私有化组件。
-   纯Java后端分析程序，提供RESTful服务，可为其他语言探针提供分析能力。
-   高性能纯流式分析。

SkyWalking的架构图如下。

![SkyWalking架构图](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7112659951/p97734.png)

SkyWalking的核心在于数据分析和度量结果的存储平台部分，通过HTTP或gRPC方式向SkyWalking Collector提交分析和度量数据。SkyWalking Collector对数据进行分析和聚合，存储到Elasticsearch、H2、MySQL、TiDB等其一即可，最后通过SkyWalking UI的可视化界面查看分析结果。Skywalking支持从多个来源和多种格式收集数据，支持多种语言的Skywalking Agent 、Zipkin v1/v2 、Istio勘测、Envoy度量等数据格式。

**说明：** 本文介绍SkyWalking与阿里云Elasticsearch 7.4版本的集成配置，您也可以通过Skywalking客户端上报Java应用数据，详细信息，请参见[通过SkyWalking上报Java应用数据](/intl.zh-CN/准备工作/开始监控 Java 应用/通过SkyWalking上报Java应用数据.md)。SkyWalking支持的中间件和组件，请参见[SkyWalking官方文档](https://github.com/opentracing-contrib/meta)。

## 前提条件

您已完成以下操作：

-   创建阿里云Elasticsearch实例，本文使用7.4.0版本。

    具体操作步骤，请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   准备一台Linux服务器，并在服务器中安装JDK，要求JDK版本为1.8.0及以上版本。

    建议您使用阿里云ECS服务器。购买ECS服务器的方法，请参见[步骤一：创建ECS实例](/intl.zh-CN/快速入门/通过控制台创建实例（详细版）/Linux系统实例快速入门.md)。

    **说明：** 安装JDK的方式，请参见[步骤三：安装JDK](/intl.zh-CN/最佳实践/数据库同步/RDS MySQL同步/通过Canal将MySQL数据同步到阿里云Elasticsearch.md)。如果未正确安装JDK，启动SkyWalking后查看日志，可能会显示Java not found或者java-xxx: No such file or directory报错。

-   确保Linux服务器的8080、10800、11800、12800端口不被占用。
-   关闭Linux服务器的防火墙及SELinux。

## 操作流程

1.  [步骤一：下载并安装SkyWalking](#section_kkj_d78_ojy)

2.  [步骤二：配置SkyWalking与Elasticsearch连通](#section_2u5_xqx_gha)

3.  [步骤三：验证结果](#section_dda_3ss_zv5)


## 步骤一：下载并安装SkyWalking

1.  在Linux服务器中，下载[SkyWalking](http://skywalking.apache.org/downloads/)。

    建议选择最新的7.0.0版本。由于本文使用的是Elasticsearch 7.4.0版本，因此选择Binary Distribution for ElasticSearch 7二进制包。下载命令如下。

    ```
    wget https://archive.apache.org/dist/skywalking/7.0.0/apache-skywalking-apm-es7-7.0.0.tar.gz
    ```

2.  解压。

    ```
    tar -zxvf apache-skywalking-apm-es7-7.0.0.tar.gz
    ```

3.  查看解压后的文件。

    ```
    ll apache-skywalking-apm-bin-es7/
    ```

    返回结果如下。

    ```
    total 92
    drwxrwxr-x 8 1001 1002   143 Mar 18 23:50 agent
    drwxr-xr-x 2 root root   241 Apr 10 16:03 bin
    drwxr-xr-x 2 root root   221 Apr 10 16:03 config
    -rwxrwxr-x 1 1001 1002 29791 Mar 18 23:37 LICENSE
    drwxrwxr-x 3 1001 1002  4096 Apr 10 16:03 licenses
    -rwxrwxr-x 1 1001 1002 32838 Mar 18 23:37 NOTICE
    drwxrwxr-x 2 1001 1002 12288 Mar 19 00:00 oap-libs
    -rw-rw-r-- 1 1001 1002  1978 Mar 18 23:37 README.txt
    drwxr-xr-x 3 root root    30 Apr 10 16:03 tools
    drwxr-xr-x 2 root root    53 Apr 10 16:03 webapp
    ```


## 步骤二：配置SkyWalking与Elasticsearch连通

1.  在config目录下，打开application.yml文件。

    ```
    cd apache-skywalking-apm-bin-es7/config/
    vi application.yml
    ```

2.  定位到`storage`部分，将默认的H2存储库改为elasticsearch7，并按照以下说明配置。

    ```
    storage:
      selector: ${SW_STORAGE:elasticsearch7}
      elasticsearch7:
        nameSpace: ${SW_NAMESPACE:"skywalking-index"}
        clusterNodes: ${SW_STORAGE_ES_CLUSTER_NODES:es-cn-4591kzdzk000i****.public.elasticsearch.aliyuncs.com:9200}
        protocol: ${SW_STORAGE_ES_HTTP_PROTOCOL:"http"}
       # trustStorePath: ${SW_SW_STORAGE_ES_SSL_JKS_PATH:"../es_keystore.jks"}
       # trustStorePass: ${SW_SW_STORAGE_ES_SSL_JKS_PASS:""}
        enablePackedDownsampling: ${SW_STORAGE_ENABLE_PACKED_DOWNSAMPLING:true} # Hour and Day metrics will be merged into minute index.
        dayStep: ${SW_STORAGE_DAY_STEP:1} # Represent the number of days in the one minute/hour/day index.
        user: ${SW_ES_USER:"elastic"}
        password: ${SW_ES_PASSWORD:"es_password"}
    ```

    **说明：** SkyWalking服务默认使用H2存储，不具有持久存储的特性，所以需要将存储组件修改为elasticsearch。

    |参数|说明|
    |--|--|
    |selector|存储选择器。本文设置为elasticsearch7。|
    |nameSpace|命名空间。Elasticsearch实例中，所有索引的命名会使用此参数值作为前缀。|
    |clusterNodes|指定Elasticsearch实例的访问地址。由于实例与SkyWalking不在同一专有网络VPC（Virtual Private Cloud）下，因此要使用公网访问地址，获取方式请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。|
    |user|Elasticsearch实例的访问用户名，默认为elastic。|
    |password|对应用户的密码。elastic用户的密码在创建实例时指定，如果忘记可重置。重置密码的注意事项和操作步骤，请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。|

    **说明：** 配置中仅指定用户名和密码即可，请注释trustStorePath和trustStorePass，否则会报错NoSuchFileException:../es\_keystore.jks。

3.  修改监听的IP地址或端口号。

    SkyWalking默认使用12800作为Rest API通信端口，11800为gRPC API端口，可在application.yml文件的core中修改，本文使用默认配置。

    ```
    core:
      selector: ${SW_CORE:default}
      default:
        # Mixed: Receive agent data, Level 1 aggregate, Level 2 aggregate
        # Receiver: Receive agent data, Level 1 aggregate
        # Aggregator: Level 2 aggregate
        role: ${SW_CORE_ROLE:Mixed} # Mixed/Receiver/Aggregator
        restHost: ${SW_CORE_REST_HOST:0.0.0.0}
        restPort: ${SW_CORE_REST_PORT:12800}
        restContextPath: ${SW_CORE_REST_CONTEXT_PATH:/}
        gRPCHost: ${SW_CORE_GRPC_HOST:0.0.0.0}
        gRPCPort: ${SW_CORE_GRPC_PORT:11800}
    ```

4.  在webapp目录下，修改webapp.yml配置。

    本文使用默认配置，您也可以根据具体需求修改。

    ```
    server:
      port: 8080
    collector:
      path: /graphql
      ribbon:
        ReadTimeout: 10000
        # Point to all backend's restHost:restPort, split by ,
        listOfServers: 127.0.0.1:12800
    ```


## 步骤三：验证结果

1.  在Linux服务器中，启动SkyWalking。

    ```
    cd ../bin
    ./startup.sh
    ```

    **说明：**

    -   在启动SkyWalking前，请确保Elasticsearch实例为正常状态。
    -   执行`startup.sh`命令，会同时启动Collector和UI。
    启动成功后，返回如下结果。

    ```
    SkyWalking OAP started successfully!
    SkyWalking Web Application started successfully!
    ```

2.  在浏览器中，访问http://<Linux服务器的IP地址\>:8080/。

    ![访问skywalking](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7112659951/p97771.png)

    **说明：** 初次使用SkyWalking连接Elasticsearch服务，启动会比较慢。因为SkyWalking需要向Elasticsearch服务创建大量的index，所以在未创建完成之前，访问这个页面会显示空白。此时您可以通过查看日志来判断启动是否完成，日志路径为`<SkyWalking的安装路径>logs/skywalking-oap-server.log`。

3.  参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)，登录对应Elasticsearch实例的Kibana控制台，执行`GET _cat/indices?v`命令查看索引数据。

    根据返回结果，可以看到Elasticsearch实例中包含了大量以`skywalking-index`开头的索引。

    ![skywalking-index开头的索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7112659951/p97778.png)


