---
keyword: [Monstache实时数据同步, MongoDB与ES数据同步]
---

# 通过Monstache实时同步MongoDB数据到阿里云ES

当您的业务数据存储在MongoDB中，并且需要进行语义分析和大图展示时，可借助阿里云Elasticsearch（简称ES）实现全文搜索、语义分析、可视化展示等。本文介绍如何通过Monstache将MongoDB数据实时同步至阿里云ES，并对数据进行分析及展示。

本文以解析及统计热门电影数据为例，提供的解决方案可以帮助您完成以下需求：

-   通过Monstache快速同步及订阅全量或增量数据。
-   将MongoDB数据实时同步至高版本ES。
-   解读Monstache常用配置参数，应用于更多的业务场景。

## 方案优势

-   MongoDB、阿里云ES及Monstache服务部署在专有网络VPC（Virtual Private Cloud）内，所有数据私网通信，高速且安全。
-   Monstache基于MongoDB的oplog实现实时数据同步及订阅，支持MongoDB与高版本ES之间的数据同步，同时支持MongoDB的变更流和聚合管道功能，并且拥有丰富的[特性](https://rwynn.github.io/monstache-site/)。
-   Monstache不仅支持软删除和硬删除，还支持数据库删除和集合删除，能够确保ES端实时与源端数据保持一致。

## 操作流程

1.  [准备工作](#section_jqd_7jz_x4p)

    准备同一专有网络VPC下的阿里云MongoDB实例、阿里云ES实例和ECS实例。其中ECS实例用来安装Monstache。

    **说明：** 请准备版本兼容的Monstache工具、阿里云ES和MongoDB实例，版本兼容性详情请参见[Monstache version](https://rwynn.github.io/monstache-site/start/)。

2.  [步骤一：搭建Monstache环境](#section_5ti_1i2_n93)

    在ECS实例中安装Monstache，用来将MongoDB中的数据同步至阿里云ES。安装前需要先配置Go环境变量。

3.  [步骤二：配置实时同步任务](#section_q8c_w5h_4di)

    修改默认的Monstache配置文件，在配置文件中指定MongoDB和ES的访问地址、待同步的集合、ES的用户名和密码等参数值。配置完成后，运行Monstache服务，即可将MongoDB中的数据实时同步至阿里云ES中。

4.  [步骤三：验证结果](#section_6ug_64d_cp6)

    分别在MongoDB数据库中添加、更新、删除数据，验证数据是否实时同步。

5.  [步骤四：通过Kibana分析并展示数据](#section_yuq_jwr_tc9)

    在Kibana控制台中，分析数据并使用Pie图展示分析结果。


## 准备工作

1.  创建阿里云ES实例，并开启实例的自动创建索引功能。

    具体操作步骤请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。本文使用的实例版本为通用商业版6.7.0。

2.  创建阿里云MongoDB实例，并准备测试数据。

    具体操作步骤请参见[MongoDB快速入门](/cn.zh-CN/快速入门/创建实例/创建副本集实例.md)。本文以4.2版本的副本集MongoDB实例为例，部分数据如下。

    ![测试数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1602659951/p128772.png)

    **说明：** MongoDB实例必须是副本集或分片集架构，不支持单节点架构。

3.  创建ECS实例。

    具体操作步骤请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。该ECS实例用来安装Monstache，需要与阿里云ES实例在同一VPC下。


## 步骤一：搭建Monstache环境

1.  连接ECS实例。

    具体操作步骤请参见[连接ECS实例]()。

2.  安装Go，并配置环境变量。

    **说明：** 由于Monstache数据同步依赖于Go语言，因此需要先在ECS中准备Go环境。

    1.  下载Go安装包并解压。

        ```
        wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
        tar -C /usr/local -xzf go1.14.4.linux-amd64.tar.gz
        ```

    2.  配置环境变量。

        使用`vim /etc/profile`命令打开环境变量配置文件，并将如下内容写入该文件中。其中`GOPROXY`用来指定阿里云Go模块代理。

        ```
        export GOROOT=/usr/local/go
        export GOPATH=/home/go/
        export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
        export GOPROXY=https://mirrors.aliyun.com/goproxy/
        ```

    3.  应用环境变量配置。

        ```
        source /etc/profile
        ```

3.  安装Monstache。

    1.  进入安装路径。

        ```
        cd /usr/local/
        ```

    2.  从Git库中下载安装包。

        ```
        git clone https://github.com/rwynn/monstache.git
        ```

    3.  进入monstache目录。

        ```
        cd monstache
        ```

    4.  切换版本。

        本文以rel5版本为例。

        ```
        git checkout rel5
        ```

    5.  安装Monstache。

        ```
        go install
        ```

    6.  查看Monstache版本。

        ```
        monstache -v
        ```

        执行成功后，返回如下结果。

        ```
        5.5.5
        ```


## 步骤二：配置实时同步任务

[Monstache配置](https://rwynn.github.io/monstache-site/start/#usage)使用TOML格式，默认情况下，Monstache会使用默认端口连接本地主机上的ES和MongoDB，并追踪MongoDB oplog。在Monstache运行期间，MongoDB的任何更改都会同步到ES中。

由于本文使用阿里云MongoDB和ES，并且需要指定同步对象（mydb数据库中的hotmovies和col集合），因此要修改默认的Monstache配置文件。修改方式如下：

1.  进入Monstache安装目录，创建并编辑配置文件。

    ```
    cd /usr/local/monstache/
    vim config.toml
    ```

2.  参考以下示例，修改配置文件。

    简单的配置示例如下，详细配置请参见[Monstache Usage](https://rwynn.github.io/monstache-site/start/#usage)。

    ```
    # connection settings
    
    # connect to MongoDB using the following URL
    mongo-url = "mongodb://root:<your_mongodb_password>@dds-bp1aadcc629******.mongodb.rds.aliyuncs.com:3717"
    # connect to the Elasticsearch REST API at the following node URLs
    elasticsearch-urls = ["http://es-cn-mp91kzb8m00******.elasticsearch.aliyuncs.com:9200"]
    
    # frequently required settings
    
    # if you need to seed an index from a collection and not just listen and sync changes events
    # you can copy entire collections or views from MongoDB to Elasticsearch
    direct-read-namespaces = ["mydb.hotmovies","mydb.col"]
    
    # if you want to use MongoDB change streams instead of legacy oplog tailing use change-stream-namespaces
    # change streams require at least MongoDB API 3.6+
    # if you have MongoDB 4+ you can listen for changes to an entire database or entire deployment
    # in this case you usually don't need regexes in your config to filter collections unless you target the deployment.
    # to listen to an entire db use only the database name.  For a deployment use an empty string.
    #change-stream-namespaces = ["mydb.col"]
    
    # additional settings
    
    # if you don't want to listen for changes to all collections in MongoDB but only a few
    # e.g. only listen for inserts, updates, deletes, and drops from mydb.mycollection
    # this setting does not initiate a copy, it is only a filter on the change event listener
    #namespace-regex = '^mydb\.col$'
    # compress requests to Elasticsearch
    #gzip = true
    # generate indexing statistics
    #stats = true
    # index statistics into Elasticsearch
    #index-stats = true
    # use the following PEM file for connections to MongoDB
    #mongo-pem-file = "/path/to/mongoCert.pem"
    # disable PEM validation
    #mongo-validate-pem-file = false
    # use the following user name for Elasticsearch basic auth
    elasticsearch-user = "elastic"
    # use the following password for Elasticsearch basic auth
    elasticsearch-password = "<your_es_password>"
    # use 4 go routines concurrently pushing documents to Elasticsearch
    elasticsearch-max-conns = 4
    # use the following PEM file to connections to Elasticsearch
    #elasticsearch-pem-file = "/path/to/elasticCert.pem"
    # validate connections to Elasticsearch
    #elastic-validate-pem-file = true
    # propogate dropped collections in MongoDB as index deletes in Elasticsearch
    dropped-collections = true
    # propogate dropped databases in MongoDB as index deletes in Elasticsearch
    dropped-databases = true
    # do not start processing at the beginning of the MongoDB oplog
    # if you set the replay to true you may see version conflict messages
    # in the log if you had synced previously. This just means that you are replaying old docs which are already
    # in Elasticsearch with a newer version. Elasticsearch is preventing the old docs from overwriting new ones.
    #replay = false
    # resume processing from a timestamp saved in a previous run
    resume = true
    # do not validate that progress timestamps have been saved
    #resume-write-unsafe = false
    # override the name under which resume state is saved
    #resume-name = "default"
    # use a custom resume strategy (tokens) instead of the default strategy (timestamps)
    # tokens work with MongoDB API 3.6+ while timestamps work only with MongoDB API 4.0+
    resume-strategy = 0
    # exclude documents whose namespace matches the following pattern
    #namespace-exclude-regex = '^mydb\.ignorecollection$'
    # turn on indexing of GridFS file content
    #index-files = true
    # turn on search result highlighting of GridFS content
    #file-highlighting = true
    # index GridFS files inserted into the following collections
    #file-namespaces = ["users.fs.files"]
    # print detailed information including request traces
    verbose = true
    # enable clustering mode
    cluster-name = 'es-cn-mp91kzb8m00******'
    # do not exit after full-sync, rather continue tailing the oplog
    #exit-after-direct-reads = false
    [[mapping]]
    namespace = "mydb.hotmovies"
    index = "hotmovies"
    type = "movies"
    
    [[mapping]]
    namespace = "mydb.col"
    index = "mydbcol"
    type = "collection"
    ```

    |参数|说明|
    |--|--|
    |mongo-url|MongoDB实例的主节点访问地址。可在实例的基本信息页面获取，获取前需配置MongoDB实例的白名单，即在白名单中添加安装Monstache的ECS实例的内网IP地址，详情请参见[设置白名单]()。|
    |elasticsearch-urls|阿里云ES实例的访问地址，格式为`http://<阿里云ES实例的内网地址>:9200`。阿里云ES实例的内网地址可在实例的基本信息页面获取，详情请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。|
    |direct-read-namespaces|指定待同步的集合，详情请参见[direct-read-namespaces](https://rwynn.github.io/monstache-site/config/#direct-read-namespaces)。本文同步的数据集为mydb数据库下的hotmovies和col集合。|
    |change-stream-namespaces|如果要使用MongoDB变更流功能，需要指定此参数。启用此参数后，oplog追踪会被设置为无效，详情请参见[change-stream-namespaces](https://rwynn.github.io/monstache-site/config/#change-stream-namespaces)。|
    |namespace-regex|通过正则表达式指定需要监听的集合。此设置可以用来监控符合正则表达式的集合中数据的变化。|
    |elasticsearch-user|访问阿里云ES实例的用户名，默认为elastic。 **说明：** 实际业务中不建议使用elastic用户，这样会降低系统安全性。建议使用自建用户，并给予自建用户分配相应的角色和权限，详情请参见[创建角色](/cn.zh-CN/访问控制/Kibana角色管理/创建角色.md)和[创建用户](/cn.zh-CN/访问控制/Kibana角色管理/创建用户.md)。 |
    |elasticsearch-password|对应用户的密码。elastic用户的密码在创建实例时指定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |elasticsearch-max-conns|定义连接ES的线程数。默认为4，即使用4个Go线程同时将数据同步到ES。|
    |dropped-collections|默认为true，表示当删除MongoDB集合时，会同时删除ES中对应的索引。|
    |dropped-databases|默认为true，表示当删除MongoDB数据库时，会同时删除ES中对应的索引。|
    |resume|默认为false。设置为true，Monstache会将已成功同步到ES的MongoDB操作的时间戳写入monstache.monstache集合中。当Monstache因为意外停止时，可通过该时间戳恢复同步任务，避免数据丢失。如果指定了cluster-name，该参数将自动开启，详情请参见[resume](https://rwynn.github.io/monstache-site/config/#resume)。|
    |resume-strategy|指定恢复策略。仅当resume为true时生效，详情请参见[resume-strategy](https://rwynn.github.io/monstache-site/config/#resume-strategy)。|
    |verbose|默认为false，表示不启用调试日志。|
    |cluster-name|指定集群名称。指定后，Monstache将进入高可用模式，集群名称相同的进程将进行协调，详情请参见[cluster-name](https://rwynn.github.io/monstache-site/config/#cluster-name)。|
    |mapping|指定ES索引映射。默认情况下，数据从MongoDB同步到ES时，索引会自动映射为`数据库名.集合名`。如果需要修改索引名称，可通过该参数设置，详情请参见[Index Mapping](https://rwynn.github.io/monstache-site/advanced/#index-mapping)。|

    **说明：** Monstache支持丰富的参数配置，以上配置仅使用了部分参数完成数据实时同步，如果您有更复杂的同步需求，请参见[Monstache config](https://rwynn.github.io/monstache-site/config/#configuration)和[Advanced](https://rwynn.github.io/monstache-site/advanced/#index-mapping)进行配置。

3.  运行Monstache。

    ```
    monstache -f config.toml
    ```

    **说明：** 通过-f参数，您可以显式运行Monstache，系统会打印所有调试日志（包括对ES的请求追踪）。


## 步骤三：验证结果

1.  分别进入MongoDB的DMS控制台和阿里云ES实例的Kibana控制台，查看同步前后对应文档的数量。

    **说明：**

    -   登录DMS控制台的方法请参见[通过DMS连接MongoDB副本集实例](/cn.zh-CN/快速入门/连接实例/通过DMS连接MongoDB副本集实例.md)。
    -   登录Kibana控制台的方法请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。
    -   MongoDB

        ```
        db.hotmovies.find().count()
        ```

        执行成功后，返回如下结果。

        ```
        [
        10000
        ]
        ```

    -   阿里云ES

        ```
        GET hotmovies/_count
        ```

        执行成功后，返回如下结果。

        ```
        {
          "count" : 10000,
          "_shards" : {
            "total" : 5,
            "successful" : 5,
            "skipped" : 0,
            "failed" : 0
          }
        }
        ```

2.  在MongoDB数据库中插入数据，查看该数据是否同步到阿里云ES实例中。

    -   MongoDB

        ```
        db.hotmovies.insert({id: 11003,title: "乘风破浪的程序媛",overview: "一群IT高智商女人，如何打破传统逆序IT精英",original_language:"cn",release_date:"2020-06-17",popularity:67.654,vote_count:65487,vote_average:9.9})
        ```

        ```
        db.hotmovies.insert({id: 11004,title: "英姿飒爽的程序猿",overview: "一群IT高智商man，如何打破传统逆序IT精英",original_language:"cn",release_date:"2020-06-15",popularity:77.654,vote_count:85487,vote_average:11.9})
        ```

    -   阿里云ES

        ```
        GET hotmovies/_search
        {
          "query": {
            "bool": {
              "should": [
                {"term":{"id":"11003"}},
                {"term":{"id":"11004"}}
              ]
            }
          }
        }
        ```

        ![插入数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1602659951/p128730.png)

3.  在MongoDB数据库中更新数据，查看阿里云ES实例中对应的数据是否会同步更新。

    -   MongoDB

        ```
        db.hotmovies.update({'title':'乘风破浪的程序媛'},{$set:{'title':'美女小姐姐'}})
        ```

    -   阿里云ES

        ```
        GET hotmovies/_search
        {
          "query": {
            "match": {
              "id":"11003"
            }
          }
        }
        ```

        ![更新数据返回结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1602659951/p128722.png)

4.  在MongoDB数据库中删除数据，查看阿里云ES实例中对应的数据是否会同步删除。

    -   MongoDB

        ```
        db.hotmovies.remove({id: 11003})
        ```

        ```
        db.hotmovies.remove({id: 11004})
        ```

    -   阿里云ES

        ```
        GET hotmovies/_search
        {
          "query": {
            "bool": {
              "should": [
                {"term":{"id":"11003"}},
                {"term":{"id":"11004"}}
              ]
            }
          }
        }
        ```

        ![删除数据返回结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1602659951/p128733.png)


## 步骤四：通过Kibana分析并展示数据

1.  登录目标阿里云ES实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  创建索引模式。

    ![创建索引模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1602659951/p128741.png)

    1.  在左侧导航栏，单击**Management**。

    2.  在**Kibana**区域，单击**Index Patterns**。

    3.  单击**Create index pattern**。

    4.  输入**Index pattern**名称，单击**Next step**。

    5.  从**Time Filter field name**中，选择时间过滤器字段名（本文选择**I don't want to use the Time Filter**）。

    6.  单击**Create index pattern**。

3.  配置Kibana大图。

    本文以配置最受欢迎的Top10电影的Pie图为例，操作步骤如下：

    1.  在左侧导航栏，单击**Visualize**。

    2.  在搜索框右侧，单击**+**。

    3.  在**New Visualization**对话框中，单击**Pie**。

        ![创建Pie图](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1602659951/p128759.png)

    4.  单击**hotmovies**索引模式。

        ![单击索引模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1602659951/p128766.png)

    5.  按照下图配置**Metrics**和**Buckets**。

        ![Pie图配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2602659951/p128764.png)

    6.  单击![运行图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2602659951/p128763.png)图标，应用配置，查看数据展示结果。

        ![Pie图展示结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2602659951/p128765.png)


## 常见问题

问题：阿里云ES实例开启高可用、高并发功能后，数据有丢失现象，要如何排查？

解决方案：查看阿里云ES集群的整体情况是否正常，如果阿里云ES集群状态正常，则需要排查Monstache服务的问题，详情请参见[Monstache官网](https://rwynn.github.io/monstache-site/)；如果阿里云ES集群状态异常，则需要排查阿里云ES集群的问题，其中阿里云ES集群的常见问题及解决方案，详情请参见[热点文章](/cn.zh-CN/.md)，同时降低并发数，观察是否正常。如果您的问题仍未解决，可以[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)咨询。

