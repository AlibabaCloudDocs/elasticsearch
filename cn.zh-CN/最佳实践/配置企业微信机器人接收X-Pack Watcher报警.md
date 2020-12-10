# 配置企业微信机器人接收X-Pack Watcher报警

通过为阿里云Elasticsearch添加X-Pack Watcher，可以实现当满足某些条件时执行某些操作。例如当logs索引中出现error日志时，触发系统自动发送报警邮件或机器人消息。可以简单地理解为X-Pack Watcher是一个基于Elasticsearch实现的监控报警服务。本文介绍如何配置企业微信机器人接收X-Pack Watcher报警。

## 注意事项

因阿里云Elasticsearch网络架构调整，2020年10月起创建的实例暂时不支持Watcher、LDAP认证、跨集群Reindex、跨集群搜索、实例网络互通功能，待后期功能上线后开放，请耐心等待。

## 前提条件

-   创建单可用区的阿里云Elasticsearch实例。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

    **说明：** X-Pack Watcher功能仅支持单可用区的Elasticsearch实例，不支持多可用区实例。

-   开启Elasticsearch实例的X-Pack Watcher功能（默认关闭）。

    具体操作，请参见[配置YML参数](/cn.zh-CN/ES实例/集群配置/配置YML参数.md)。

-   购买ECS实例。

    ECS实例要与Elasticsearch实例在同一个区域和专有网络下，并且能够访问公网。具体操作，请参见[步骤一：创建ECS实例](/cn.zh-CN/快速入门/通过控制台使用ECS实例（详细版）/Linux系统实例快速入门.md)。

    **说明：** 阿里云Elasticsearch的X-Pack Watcher功能不支持直接与公网通讯，需要基于实例的内网地址通讯（专有网络VPC环境），因此您需要购买一台能同时访问公网和Elasticsearch实例的ECS实例，作为代理去执行Actions。


## 背景信息

X-Pack Watcher功能主要由Trigger、Input、Condition和Actions组成：

-   Trigger

    确定何时检查，在配置Watcher时必须设置。支持多种调度触发器，详情请参见[Schedule Trigger](https://www.elastic.co/guide/en/x-pack/5.5/trigger-schedule.html)。

-   Input

    需要对监控的索引执行的筛选条件，详情请参见[Inputs](https://www.elastic.co/guide/en/x-pack/5.5/input.html)。

-   Condition

    执行Actions的条件。

-   Actions

    当条件发生时，执行的具体操作。本文以配置Webhook Action为例。


## 操作步骤

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
        |**授权对象**|添加您购买的阿里云Elasticsearch实例所有节点的IP地址。 **说明：** 参见[查看节点信息](/cn.zh-CN/ES实例/实例管理/查看可视化节点信息.md)，获取Elasticsearch实例中所有节点的IP地址。 |
        |**描述**|输入对规则的描述。|

    6.  单击**保存**。

2.  配置Nginx代理。

    1.  在ECS上安装Nginx。

        具体安装方法请参见[Nginx安装配置](http://www.runoob.com/linux/nginx-install-setup.html)。

    2.  配置nginx代理转发。

        使用以下配置替换nginx.conf文件中`server`部分的配置。

        ```
        server {
                listen 8080;
                server_name _;
                root /usr/share/nginx/html;
                # Load configuration files for the default server block.
                include /etc/nginx/default.d/*.conf;
        
                  location / {
                    proxy_pass <企业微信机器人Webhook地址>;
                  }
                error_page 404 /404.html;
                    location = /40x.html {
                }
                error_page 500 502 503 504 /50x.html;
                    location = /50x.html {
                }
            }
        ```

        <企业微信机器人Webhook地址\>：请替换为接收报警消息的企业微信机器人的Webhook地址。

    3.  加载修改后的配置文件并重启Nginx。

        ```
        /usr/local/webserver/nginx/sbin/nginx -s reload            # 重新载入配置文件
        /usr/local/webserver/nginx/sbin/nginx -s reopen            # 重启Nginx
        ```

3.  设置X-Pack Watcher报警规则。

    1.  登录对应阿里云Elasticsearch实例的Kibana控制台。

        具体操作，请参见[登录Kibana控制台](/cn.zh-CN/ES实例/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  在左侧菜单栏，单击**Dev Tools**（开发工具）。

    3.  在**Console**中，执行如下命令创建一个报警文档。

        以下示例以创建`developer_count_watch`为例，每隔`10s`查询`zl-testgaes`索引中是否出现`developer`字段，如果出现`158974`次以上则触发报警。

        ```
        PUT _xpack/watcher/watch/developer_count_watch
        {
          "trigger": {
            "schedule": {
              "interval": "10s"
            }
          },
          "input": {
            "search": {
              "request": {
                "indices": ["zl-testgaes"],
                "body": {
                  "query": {
            "bool": {
              "must": [
                {"match": 
                 {
                   "developer" : "Nintendo"    
                }
                },
                {
                "range": {
                  "year_of_release": {
                    "gte": "2011-09-20T16:00:00.000Z",
                    "lte": "2011-12-31T16:00:00.000Z"
                          }
                      }
                }
              ]
            } 
          }
                }
              }
            }
          },
          "condition": {
            "compare": {
              "ctx.payload.hits.total": {
                "gt": 158974
              }
            }
          },
          "actions" : {
          "test_issue" : {
            "webhook" : {
              "method" : "POST",
              "url" : "http://<ECS的内网IP地址>:8080",
              "body" : "{\"msgtype\": \"text\", \"text\": { \"content\": \"developer is Nintendo，More than 158974\"}}"
            }
          }
        }
        }
        ```

        触发报警后，企业微信机器人将收到如下报警。

        ![企业微信机器人报警配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6706320061/p166882.png)


