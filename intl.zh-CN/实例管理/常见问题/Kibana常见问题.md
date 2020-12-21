# Kibana常见问题

本文汇总了使用阿里云Elasticsearch的Kibana控制台时的常见问题。

## 如何登录Kibana控制台，用户名和密码是什么？

登录Kibana控制台的具体步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。Kibana控制台的用户名默认为elastic，密码为您创建阿里云Elasticsearch实例时设置的密码。如果忘记密码，可重置，重置前的注意事项以及重置密码的具体步骤，请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。

## Kibana控制台的elastic账号的密码有什么作用？

**说明：** elastic账号是Elasticsearch服务的管理员账号，拥有集群管理最高权限。

通过以下方式访问Elasticsearch实例时，需要使用elastic账号的密码进行权限校验：

-   通过API及SDK访问实例。
-   通过Kibana服务访问实例。

## 我可以在Kibana控制台中，访问公网中的服务吗（例如百度地图、高德地图等）？

不可以。只能访问专有网络VPC（Virtual Private Cloud）内的服务。

## 如何在Kibana控制台中更好地管理权限？

-   建议您在Kibana控制台中，创建新用户并分配角色权限，避免直接使用elastic账号（管理员账号）操作实例，详情请参见[索引操作权限](/intl.zh-CN/访问控制/Kibana角色管理/创建角色.md)。
-   建议您不要在搜索业务中使用elastic账号。因为elastic账号的密码泄露后，可能会导致您的集群存在安全风险。
-   请谨慎变更elastic账号的密码。如果您在业务中使用elastic账号提供服务，重置密码后，业务将会因请求鉴权失败出现不可用的状态。

## Kibana无法启动，登录时报错Kibana server is not ready yet，如何处理？

原因：删除了系统索引或Kibana节点丢失。

解决方法：

-   删除了系统索引：通过快照恢复删除的系统索引。具体操作步骤，请参见[自动备份与恢复](/intl.zh-CN/实例管理/数据备份/自动备份与恢复.md)。
-   Kibana节点丢失：删除.kibana\_1、.kibana相关的其他索引，然后通过控制台重启Kibana节点，或者重启Elasticsearch服务。具体操作步骤，请参见[重启实例或节点](/intl.zh-CN/实例管理/管理实例/重启实例或节点.md)。

## 如何在Kibana控制台中查看分片、索引信息？

-   通过`GET _nodes/stats`命令查看索引信息。

    ![通过命令查看索引和分片信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5011805061/p181570.png)

-   在**Monitoring**页面下，查看某个节点下索引的分片情况（包括堆内存使用情况），如下图所示。

    ![查看分片信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5011805061/p181569.png)


## Kibana控制台中，通过elastic账号创建子账号时，提示You do not have permission to manage users，如何处理？

报错截图如下。

![报错截图](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5011805061/p181589.png)

解决方案：

1.  通过Kibana控制台查看证书是否过期。

    ```
    GET _license
    ```

    -   是：提交工单，联系阿里云Elasticsearch技术支持工程师处理。
    -   否：继续执行下一步。
2.  通过`GET /_cat/indices?v`命令，查看集群中是否存在多个系统索引.security-\*。
    -   是：存在多个.security-\*索引，说明您进行过全量索引迁移或同步操作，删除低版本的.security-\*索引即可。
    -   否：提交工单，联系阿里云Elasticsearch技术支持工程师处理。

## Kibana支持安装自定义插件吗？

不支持。对于7.0以下版本的Kibana，只支持控制台中提供的默认插件，7.0及以上版本不支持任何插件。

