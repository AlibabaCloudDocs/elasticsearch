---
keyword: [X-Pack Watcher, Elasticsearch monitoring and alerting service]
---

# Configure X-Pack Watcher

This topic describes how to configure X-Pack Watcher for Alibaba Cloud Elasticsearch. X-Pack Watcher allows you to trigger specific actions when specified conditions are met. For example, you can create a watch for Elasticsearch to search the logs index for errors and send alert emails or DingTalk messages. X-Pack Watcher is an Elasticsearch-based monitoring and alerting service. This topic describes how to configure X-Pack Watcher.

## Precautions

The adjustment made to the Alibaba Cloud Elasticsearch network architecture has the following impacts on clusters:

-   Clusters created after October 2020 do not support the X-Pack Watcher and Lightweight Directory Access Protocol \(LDAP\) authentication features.
-   You cannot reindex, search for, and replicate data between a cluster created before October 2020 and a cluster created after October 2020. If you want to perform these operations between them, make sure that the clusters are under the same network architecture.

**Note:** The network architecture in the China \(Zhangjiakou\) region and the regions outside China was adjusted before October 2020. If you want to perform the preceding operations between a cluster created before October 2020 and that created after October 2020 in such a region, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm) to contact technical support personnel to check whether the network architecture supports the operations.

## Prerequisites

-   A single-zone Alibaba Cloud Elasticsearch cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

    **Note:** X-Pack Watcher is available only for single-zone Elasticsearch clusters.

-   X-Pack Watcher is enabled for the Elasticsearch cluster. It is disabled by default.

    For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).

-   An Elastic Compute Service \(ECS\) instance is created.

    The ECS instance must be accessible over the Internet and reside in the same region and virtual private cloud \(VPC\) as the Elasticsearch cluster. For more information, see [Step 1: Create an ECS instance](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Linux instances.md).

    **Note:** X-Pack Watcher cannot directly access the Internet. You must use the internal endpoint of your Elasticsearch cluster to access the Internet. Therefore, you must create an ECS instance that can access both the Internet and the Elasticsearch cluster and use it as a proxy to perform actions.


## Background information

X-Pack Watcher allows you to create watches. A watch consists of a trigger, an input, a condition, and actions.

-   Trigger

    Determines when a watch starts to run. You must configure a trigger for each watch. X-Pack Watcher allows you to create various types of triggers. For more information, see [Schedule Trigger](https://www.elastic.co/guide/en/x-pack/5.5/trigger-schedule.html).

-   Input

    Loads data into the payload of a watch. Inputs are used as filters to match the specified type of index data. For more information, see [Inputs](https://www.elastic.co/guide/en/x-pack/5.5/input.html).

-   Condition

    Controls whether a watch performs actions.

-   Actions

    Determines the actions that a watch performs if the specified condition is met. The webhook action is used in this topic.


## Procedure

1.  Configure a security group rule for the ECS instance.

    1.  Log on to the [ECS console](https://ecs.console.aliyun.com). In the left-side navigation pane, click **Instances**.

    2.  On the **Instances** page, find your ECS instance and choose **More** \> **Network and Security Group** \> **Configure Security Group** in the **Actions** column.

    3.  On the **Security Groups** tab, click **Add Rules** in the **Actions** column.

    4.  On the **Inbound** tab, click **Add Security Group Rule** in the upper-right corner.

    5.  Configure parameters.

        ![Add Security Group Rule](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5667819951/p49922.png)

        |Parameter|Description|
        |---------|-----------|
        |**Action**|Select **Allow**.|
        |**Priority**|Retain the default value.|
        |**Protocol Type**|Select **Custom TCP**.|
        |**Port Range**|Set this parameter to your frequently used port. This parameter is required for NGINX configurations. In this example, port 8080 is used.|
        |**Authorization Object**|Enter the IP addresses of all nodes in your Elasticsearch cluster. **Note:** For more information about how to query the IP addresses of the nodes, see [View the basic information of nodes](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the cluster status and node information.md). |
        |**Description**|The description of the rule.|

    6.  Click **OK**.

2.  Configure an NGINX proxy.

    1.  Install NGINX on the ECS instance.

    2.  Configure the nginx.conf file.

        Replace the `server` configuration in the nginx.conf file with the following configuration:

        ![Server configuration in the nginx.conf file](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9897158061/p175712.png)

        ```
        server
          {
            listen 8080;# Listening port
            server_name localhost;# Domain name
            index index.html index.htm index.php;
            root /usr/local/webserver/nginx/html;# Website directory
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
              proxy_pass <Webhook address of the DingTalk chatbot>;
            }
            location ~ .*\.(js|css)?$
            {
              expires 15d;
              # access_log off;
            }
            access_log off;
          }
        ```

        <Webhook address of the DingTalk chatbot\>: Replace it with the webhook address of the DingTalk chatbot that is used to receive alert notifications.

        **Note:** To query the webhook address of the DingTalk chatbot, create an alert group in DingTalk. In the DingTalk group, click the Group Settings icon in the upper-right corner, Group Assistant, and then Add Robot. In the ChatBot dialog box, click Custom to add a chatbot that is accessed by using a webhook. You can then view the webhook address of the DingTalk chatbot.

    3.  Reload the NGINX configuration file and restart NGINX.

        ```
        /usr/local/webserver/nginx/sbin/nginx -s reload            # Reload the NGINX configuration file.
        /usr/local/webserver/nginx/sbin/nginx -s reopen            # Restart NGINX.
        ```

3.  Create a watch.

    1.  Log on to the Kibana console of your Elasticsearch cluster.

        **Note:** For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

    2.  In the left-side navigation pane, click **Dev Tools**.

    3.  On the **Console** tab of the page that appears, run the following command to create a watch:

        In this example, a watch named `log_error_watch` is created to search the `logs` index for `errors` every `10 seconds`. If more than `0` errors are found, an alert is triggered.

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
              "url" : "http://<Private IP address of your ECS instance>:8080",
              "body" : "{\"msgtype\": \"text\", \"text\": { \"content\": \"An error is found. Handle the issue immediately.\"}}"
            }
          }
        }
        }
        ```

        **Note:**

        -   `url` specified in `actions` must contain the private IP address of your ECS instance that resides in the same region and VPC as your Elasticsearch cluster. You must also create a security group rule for the ECS instance. Otherwise, the instance cannot be used to access the Elasticsearch cluster.
        -   If the error `No handler found for uri [/_xpack/watcher/watch/log_error_watch_2] and method [PUT]` is returned after you run the preceding command, X-Pack Watcher is disabled for your Elasticsearch cluster. In this case, enable X-Pack Watcher and run the command again. For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).
        -   When you create a DingTalk chatbot, you must configure security settings. The body parameter in the preceding code needs to be configured based on the security settings. For more information, see [Configure security settings](https://ding-doc.dingtalk.com/doc#/serverapi2/qf2nxq). In this topic, **Security Settings** is set to **Custom Keywords** and the error keyword is specified. In this case, the DingTalk chatbot sends alert notifications when the content field in the body parameter contains error.
        If you no longer require this watch, run the following command to delete the watch:

        ```
        DELETE _xpack/watcher/watch/log_error_watch
        ```


