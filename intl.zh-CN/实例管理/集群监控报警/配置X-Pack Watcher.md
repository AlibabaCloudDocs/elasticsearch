---
keyword: [x-pack watcher, es监控报警服务]
---

# 配置X-Pack Watcher

通过为阿里云Elasticsearch添加X-Pack Watcher，可以实现当满足某些条件时执行某些操作。例如当logs索引中出现error日志时，触发系统自动发送报警邮件或钉钉消息。可以简单地理解为X-Pack Watcher是一个基于Elasticsearch实现的监控报警服务。

-   已经创建了单可用区的阿里云Elasticsearch实例。

    详情请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

    **说明：** X-Pack Watcher功能仅支持单可用区的阿里云Elasticsearch实例，不支持多可用区实例。

-   已经开启了阿里云Elasticsearch实例的X-Pack Watcher功能（默认关闭）。

    详情请参见[配置YML参数](/intl.zh-CN/实例管理/ES集群配置/修改YML参数配置.md)。

-   已经购买了阿里云ECS实例。

    购买的ECS实例要与阿里云Elasticsearch实例在同一个区域和专有网络VPC（Virtual Private Cloud）下，并且能够访问公网，详情请参见[步骤一：创建ECS实例](/intl.zh-CN/快速入门/通过控制台创建实例（详细版）/Linux系统实例快速入门.md)。

    **说明：** 阿里云Elasticsearch的X-Pack Watcher功能不支持直接与公网进行通讯，需要基于实例的内网地址来进行通讯（专有网络VPC环境），因此您需要购买一台能同时访问公网和Elasticsearch实例的ECS实例，作为代理去执行Actions。


X-Pack Watcher功能主要由Trigger、Input、Condition和Actions组成：

-   Trigger

    确定何时检查，在配置Watcher时必须设置。支持多种调度触发器，详情请参见[Schedule Trigger](https://www.elastic.co/guide/en/x-pack/5.5/trigger-schedule.html)。

-   Input

    需要对监控的索引执行的筛选条件，详情请参见[Inputs](https://www.elastic.co/guide/en/x-pack/5.5/input.html)。

-   Condition

    执行Actions的条件。

-   Actions

    当条件发生时，执行的具体操作。本文以配置Webhook Action为例。


1.  配置ECS安全组。

    1.  登录[阿里云ECS控制台](https://ecs.console.aliyun.com)，在左侧导航栏，单击**实例**。

    2.  在**实例列表**页面，选择对应实例右侧**操作**列下的**更多** \> **网络和安全组** \> **安全组配置**。

    3.  在**安全组列表**页签右侧的**操作**列下，单击**配置规则**。

    4.  在**入方向**页签，单击**手动添加**。

    5.  填写相关参数。

        ![添加安全组规则](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6706320061/p49922.png)

        |参数|说明|
        |--|--|
        |**授权策略**|选择**允许**。|
        |**优先级**|保持默认。|
        |**协议类型**|选择**自定义TCP**。|
        |**端口范围**|填写您常用的端口（配置Nginx时需要用到，本文以8080为例）。|
        |**授权对象**|添加您购买的阿里云Elasticsearch实例所有节点的IP地址。 **说明：** 获取Elasticsearch实例所有节点的IP地址列表：参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)，登录对应实例的Kibana控制台，单击左侧导航栏的**Monitoring**，再单击**Nodes**。 |
        |**描述**|输入对规则的描述。|

    6.  单击**保存**。

2.  配置Nginx代理。

    1.  在ECS上安装Nginx。

    2.  配置nginx.conf文件。

        使用以下配置替换nginx.conf文件中`server`部分的配置。

        ![ngnix.conf的server部分配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1463533061/p175712.png)

        ```
        server
          {
            listen 8080;#监听端口
            server_name localhost;#域名
            index index.html index.htm index.php;
            root /usr/local/webserver/nginx/html;#站点目录
              location ~ .*\.(php|php5)?$
            {
              #fastcgi_pass unix:/tmp/php-cgi.sock;
              fastcgi_pass 127.0.0.1:9000;
              fastcgi_index index.php;
              include fastcgi.conf;
            }
            location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$
            {
              expires 30d;
              # access_log off;
            }
            location / {
              proxy_pass <钉钉机器人Webhook地址>;
            }
            location ~ .*\.(js|css)?$
            {
              expires 15d;
              # access_log off;
            }
            access_log off;
          }
        ```

        `<钉钉机器人Webhook地址>`：请替换为接收报警消息的钉钉机器人的Webhook地址。

        **说明：** 获取钉钉群机器人的Webhook地址：创建一个钉钉报警接收群，在群的右上角找到群机器人，然后添加一个自定义通过Webhook接入的机器人，并获取群机器人的Webhook地址，详情请参见[获取自定义机器人Webhook](https://ding-doc.dingtalk.com/doc#/serverapi2/qf2nxq)。

    3.  加载修改后的配置文件并重启Nginx。

        ```
        /usr/local/webserver/nginx/sbin/nginx -s reload            # 重新载入配置文件
        /usr/local/webserver/nginx/sbin/nginx -s reopen            # 重启Nginx
        ```

3.  设置报警。

    1.  登录对应阿里云Elasticsearch实例的Kibana控制台。

        **说明：** 登录Kibana控制台的具体步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  在左侧菜单栏，单击**Dev Tools**（开发工具）。

    3.  在**Console**中执行如下命令创建一个报警文档。

        以下示例以创建`log_error_watch`为例，每隔`10s`查询`logs`索引中是否出现`error`日志，如果出现`0`次以上则触发报警。

        ```
        PUT _xpack/watcher/watch/log_error_watch
        {
          "trigger": {
            "schedule": {
              "interval": "10s"
            }
          },
          "input": {
            "search": {
              "request": {
                "indices": ["logs"],
                "body": {
                  "query": {
                    "match": {
                      "message": "error"
                    }
                  }
                }
              }
            }
          },
          "condition": {
            "compare": {
              "ctx.payload.hits.total": {
                "gt": 0
              }
            }
          },
          "actions" : {
          "test_issue" : {
            "webhook" : {
              "method" : "POST",
              "url" : "http://<您的ECS内网IP>:8080",
              "body" : "{\"msgtype\": \"text\", \"text\": { \"content\": \"error 日志出现了，请尽快处理\"}}"
            }
          }
        }
        }
        ```

        **说明：**

        -   `actions`中配置的`url`为阿里云ECS实例的内网IP地址。该ECS必须是与Elasticsearch实例在相同区域和专有网络VPC下，并且已经按照上文的方式配置了安全组，否则不能进行通信。
        -   如果在执行以上命令时出现`No handler found for uri [/_xpack/watcher/watch/log_error_watch_2] and method [PUT]`异常，表示您购买的阿里云Elasticsearch实例未开启X-Pack Watcher功能，请开启后再执行以上命令，详情请参见[配置YML参数](/intl.zh-CN/实例管理/ES集群配置/修改YML参数配置.md)。
        -   在创建钉钉机器人时，必须进行安全设置。以上代码中的body参数需要根据安全设置进行相应配置，详情请参见[安全设置](https://ding-doc.dingtalk.com/doc#/serverapi2/qf2nxq)。例如本文选择**安全设置**方式为**自定义关键词**，且添加了一个自定义关键词：error，那么body中的content字段必须包含error，钉钉机器人才会推送报警信息。

如果不再需要执行报警任务，请使用以下命令删除该报警任务。

```
DELETE _xpack/watcher/watch/log_error_watch
```

