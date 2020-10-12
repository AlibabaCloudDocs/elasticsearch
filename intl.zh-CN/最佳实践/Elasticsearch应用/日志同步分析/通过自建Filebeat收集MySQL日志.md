# 通过自建Filebeat收集MySQL日志

当您需要查看并分析MySQL的日志信息（例如慢日志、error日志等）时，可通过Filebeat采集MySQL日志并发送到阿里云Elasticsearch中，然后在Kibana控制台中完成可视化查询、分析和展示。本文介绍具体的实现方法。

## 操作流程

1.  [准备工作](#section_pe3_0w7_6wo)

    创建阿里云Elasticsearch和ECS实例。Elasticsearch实例用来接收Filebeat采集的MySQL日志，并提供一个Kibana控制台进行可视化查询、分析与展示；ECS实例用来安装MySQL和Filebeat。

2.  [步骤一：安装并配置MySQL](#section_hyw_06v_cxp)

    安装MySQL，并在MySQL的配置文件中开启error和慢查询日志文件配置。开启后，Filebeat才可采集到对应的日志。

3.  [步骤二：安装并配置Filebeat](#section_rmz_4e2_ttp)

    安装Filebeat，用来将MySQL中的日志采集到阿里云Elasticsearch集群中。需要启用Filebeat的MySQL模块，并在配置文件中指定Kibana和阿里云Elasticsearch的访问地址。

4.  [步骤三：使用Kibana Dashboard展示MySQL日志](#section_r8e_tyr_2tm)

    进行查询测试，并通过Kibana Dashboard展示对应的error和慢查询日志。


## 准备工作

-   创建阿里云Elasticsearch实例。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)，本文以通用商业版6.7.0版本为例。

-   创建阿里云ECS实例。

    具体操作步骤请参见[使用向导创建实例](/intl.zh-CN/实例/创建实例/使用向导创建实例.md)，本文以CentOS操作系统为例。


## 步骤一：安装并配置MySQL

1.  连接ECS实例。

    具体操作步骤请参见[通过VNC远程连接登录Linux实例](/intl.zh-CN/实例/连接实例/连接Linux实例/通过VNC远程连接登录Linux实例.md)。

2.  下载并安装MySQL源。

    ```
    wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
    rpm -ivh mysql-community-release-el7-5.noarch.rpm
    ```

3.  安装MySQL。

    ```
    yum install mysql-server
    ```

4.  启动MySQL并查看服务状态。

    ```
    systemctl start mysqld
    systemctl status mysqld
    ```

5.  在my.cnf文件中配置error日志文件和慢查询日志文件。

    **说明：** 默认情况下MySQL会禁用日志文件配置，因此您需要手动开启。您也可通过MySQL命令开启临时慢日志。

    1.  打开my.cnf文件。

        ```
        vim /etc/my.cnf
        ```

    2.  配置日志文件。

        ```
        [mysqld]
        log_queries_not_using_indexes = 1
        slow_query_log=on
        slow_query_log_file=/var/log/mysql/slow-mysql-query.log
        long_query_time=0
        
        [mysqld_safe]
        log-error=/var/log/mysql/mysqld.log
        ```

        |参数|说明|
        |--|--|
        |`log_queries_not_using_indexes`|是否将未使用索引的查询记录到慢查询日志中。1表示记录，0表示不记录。|
        |`slow_query_log`|是否开启慢查询日志。on表示开启，off表示关闭。|
        |`slow_query_log_file`|指定慢查询日志的存储路径。|
        |`long_query_time`|慢查询阈值，单位为秒。当查询时间超过设定的阈值时，MySQL会将日志写入`slow_query_log_file`指定的文件中。**说明：** 本文为方便测试，将该参数值设置为0，实际情况中请根据业务需要合理设置该参数值。 |

    3.  创建日志文件。

        ```
        mkdir /var/log/mysql
        touch /var/log/mysql/mysqld.log
        touch /var/log/mysql/slow-mysql-query.log
        ```

        **说明：** MySQL不会主动创建日志文件，所以您需要手动创建对应文件。

    4.  为所有用户授予日志文件的读写权限。

        ```
        chmod 777 /var/log/mysql/slow-mysql-query.log /var/log/mysql/mysqld.log
        ```


## 步骤二：安装并配置Filebeat

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Logs**。

3.  单击**View setup instructions**。

4.  在**Add Data to Kibana**页面，单击**MySQL logs**。

5.  在**Self managed**页面，单击**RPM**。

    **说明：** 由于本文使用的是Linux系统，因此选择**RPM**。实际情况中，请根据您的操作系统类型选择合适的安装方式。

6.  按照页面提示，在ECS中安装Filebeat。

7.  修改MySQL模块配置，分别指定待采集的error和slow日志文件。

    1.  启用MySQL模块。

        ```
        sudo filebeat modules enable mysql
        ```

    2.  打开mysql.yml文件。

        ```
        vim /etc/filebeat/modules.d/mysql.yml
        ```

    3.  修改MySQL模块配置。

        ![修改MySQL模块配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1554659951/p120545.png)

        ```
        - module: mysql
          # Error logs
          error:
            enabled: true
            var.paths: ["/var/log/mysql/mysqld.log"]
            # Set custom paths for the log files. If left empty,
            # Filebeat will choose the paths depending on your OS.
            #var.paths:
        
          # Slow logs
          slowlog:
            enabled: true
            var.paths: ["/var/log/mysql/slow-mysql-query.log"]
            # Set custom paths for the log files. If left empty,
            # Filebeat will choose the paths depending on your OS.
            #var.paths:
        ```

        |参数|说明|
        |--|--|
        |`enabled`|设置为`true`。|
        |`var.paths`|设置为对应日志文件的路径。需要与MySQL配置文件中设置的路径保持一致，详情请参见[步骤一：安装并配置MySQL](#section_hyw_06v_cxp)。|

8.  配置filebeat.yml文件。

    1.  打开filebeat.yml文件。

        ```
        vim /etc/filebeat/filebeat.yml
        ```

    2.  修改Filebeat modules配置。

        ![Filebeat modules配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1554659951/p118153.png)

        ```
        filebeat.config.modules:
          # Glob pattern for configuration loading
          path: /etc/filebeat/modules.d/mysql.yml
        
          # Set to true to enable config reloading
          reload.enabled: true
        
          # Period on which files under path should be checked for changes
          reload.period: 1s
        ```

    3.  修改Kibana配置。

        ![修改Kibana配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2554659951/p118154.png)

        ```
        setup.kibana:
        host: "https://es-cn-0pp1jxvcl000*****.kibana.elasticsearch.aliyuncs.com:5601"
        ```

        `host`：Kibana的访问地址。可在Kibana配置页面获取，详情请参见[查看Kibana公网地址](/intl.zh-CN/实例管理/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)，格式为`<Kibana公网地址>:5601`。

    4.  修改Elasticsearch output配置。

        ![修改ES配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2554659951/p118155.png)

        ```
        output.elasticsearch:
          # Array of hosts to connect to.
          hosts: ["es-cn-0pp1jxvcl000*****.elasticsearch.aliyuncs.com:9200"]
          # Optional protocol and basic auth credentials.
          #protocol: "https"
          username: "elastic"
          password: "<your_password>"
        ```

        |参数|说明|
        |--|--|
        |`hosts`|阿里云Elasticsearch的访问地址，格式为`<实例的内网或公网地址>:9200`。Elasticsearch实例的内网或公网地址可在实例的基本信息页面获取，详情请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。 **说明：** 如果ECS和Elasticsearch实例在同一专有网络VPC（Virtual Private Cloud）下，请使用内网地址；如果不在同一VPC下，请使用公网地址。使用公网地址需要配置公网地址访问白名单，详情请参见[配置ES公网或私网访问白名单](/intl.zh-CN/实例管理/安全配置/配置ES公网或私网访问白名单.md)。 |
        |`username`|阿里云Elasticsearch实例的访问用户名，默认为elastic。|
        |`password`|对应用户的密码，一般在创建实例时设定。如果忘记，可重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。|

9.  执行以下命令，将Dashboard等信息上传到Elasticsearch和Kibana中，并启用Filebeat服务。

    ```
    sudo filebeat setup
    sudo service filebeat start
    ```


## 步骤三：使用Kibana Dashboard展示MySQL日志

1.  在ECS中重启MySQL数据库，并查询任意数据进行测试。

    重启命令如下。

    ```
    systemctl restart mysqld
    ```

2.  查看日志内容。

    本文检测到的日志内容如下。

    ![慢日志内容](../images/p120563.png "慢日志内容")

    ![error日志内容](../images/p120564.png "error日志内容")

3.  登录目标Elasticsearch实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

4.  在左侧导航栏，单击**Dashboard**。

5.  在**Dashboards**列表中，单击**\[Filebeat MySQL\] Overview**。

6.  在页面右上角选择查询时间，查看对应时间段内的日志。

    ![查看日志](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2554659951/p120552.png)


