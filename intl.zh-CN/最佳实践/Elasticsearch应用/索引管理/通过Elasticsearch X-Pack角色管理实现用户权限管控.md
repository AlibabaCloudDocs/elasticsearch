# 通过Elasticsearch X-Pack角色管理实现用户权限管控

当您需要设置集群、索引、字段或其他操作的访问权限时，可以通过Elasticsearch X-Pack的RBAC（Role-based Access Control）机制，为自定义角色分配权限，并将角色分配给用户，实现权限管控。Elasticsearch提供了多种内置角色，您可以在内置角色的基础上扩展自定义角色，以满足特定需求。本文介绍几种常见的角色配置，以及如何通过角色配置实现权限管控。

-   关于Elasticsearch X-Pack RBAC机制的详细信息，请参见[User authorization](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/authorization.html#roles)。
-   Elasticsearch支持多种安全认证功能，详细信息，请参见[Elasticsearch身份认证和授权](https://www.elastic.co/cn/blog/demystifying-authentication-and-authorization-in-elasticsearch)。

## 操作流程

1.  创建角色。

    具体操作，请参见[创建角色](/intl.zh-CN/访问控制/Kibana角色管理/创建角色.md)，相关参数说明如下。

    ![填写角色信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8858997061/p199132.png)

    |参数|说明|
    |--|--|
    |**Role name**|角色名称。|
    |**Run As privileges**|扮演该角色的用户，可选。如果此处未选择，可在创建用户时，为该用户指定对应角色，具体操作，请参见[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)。|
    |**Cluster privileges**|定义集群的操作权限，例如查看集群健康度和Settings、创建快照等。详细信息，请参见[Cluster privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-cluster)。|
    |**Index privileges**|定义索引的操作权限，例如只读查看所有索引的所有字段（索引名设置为\*后，授予read索引），索引名支持[通配符（\*）及正则表达式](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/defining-roles.html#roles-indices-priv)。详细信息，请参见[Indices privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-indices)。|
    |**Kibana privileges**|定义Kibana操作权限。**说明：** Kibana 7.0以下版本仅支持[Base privileges](https://www.elastic.co/guide/en/kibana/6.7/kibana-privileges.html#kibana-privileges)，默认为所有空间授权；7.0及以上版本在Base privileges的基础上，还支持[Feature privileges](https://www.elastic.co/guide/en/kibana/7.10/kibana-privileges.html#kibana-feature-privileges)。即对Kibana特定功能授权，需要指定Kibana空间。 |

    创建角色时，需要为该角色分配对应权限。本文的角色权限配置示例如下：

    -   为普通用户授予指定索引的只读（Read）权限。设置用户仅能访问指定索引，不能进行其他操作。

        详细信息，请参见[配置索引只读权限](#section_y3r_vdm_0x4)。

    -   为普通用户授予查看所有或部分Dashboard的权限。

        详细信息，请参见[配置Dashboard操作权限](#section_gor_c8m_8cc)。

    -   为普通用户授予部分索引的读写权限，及所有集群的只读权限。例如查看集群健康度、快照及Settings，写入索引数据和更新索引Mapping等。

        详细信息，请参见[配置索引读写和集群只读权限](#section_b9o_76k_efl)。

2.  创建用户，并为该用户分配对应角色，为其授予该角色拥有的权限。

    具体操作，请参见[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)。

3.  通过自定义用户登录Kibana控制台，执行相关操作。

    具体操作，请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。


## 配置索引只读权限

-   场景描述

    为普通用户授予指定索引的只读权限。该用户可通过Kibana查询索引数据，但无权访问集群。

-   角色配置

    ![索引只读权限](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8858997061/p199647.png)

    |权限类型|权限Key|权限Value|描述|
    |----|-----|-------|--|
    |**Index privileges**|**indices**|kibana\_sample\_data\_logs|指定索引名称，支持索引全名、别名、通配符及正则表达式。详细信息，请参见[Indices Privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/defining-roles.html#roles-indices-priv)。|
    |**privileges**|read|设置索引只读权限。只读权限包括count、explain、get、mget、scripts、search、scroll等操作权限。详细信息，请参见[privileges-list-indices](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-indices)。|
    |**fileds**|\*|索引字段。\*表示索引的所有字段。|
    |**Kibana privileges**|**privileges**|read|为所有空间授予Kibana只读权限。默认为none，表示所有空间无权限访问Kibana。**说明：** Kibana 7.0以下版本仅支持[Base privileges](https://www.elastic.co/guide/en/kibana/6.7/kibana-privileges.html#kibana-privileges)，默认为所有空间授权；7.0及以上版本在Base privileges的基础上，还支持[Feature privileges](https://www.elastic.co/guide/en/kibana/7.10/kibana-privileges.html#kibana-feature-privileges)。即对Kibana特定功能授权，需要指定Kibana空间。 |

-   验证

    通过普通用户登录Kibana控制台，执行读索引命令，返回结果正常。执行写索引命令，返回未授权的错误信息。

    ![验证只读权限](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8858997061/p200448.png)


## 配置Dashboard操作权限

-   场景描述

    授予普通用户指定索引的只读权限，且可查看该索引对应的Dashboard数据。

-   角色配置

    在[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)时，为该用户分配角色：read-index和kibana\_dashboard\_only\_user。

    ![Dashboard角色配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8858997061/p199843.png)

    -   read-index：自定义角色示例，需要您自行创建。该角色拥有指定索引的只读权限。
    -   kibana\_dashboard\_only\_user：Kibana内置角色，该角色拥有查看指定索引Dashboard数据的权限。

        **说明：**

        -   在Kibana 7.0及以上版本中，kibana\_dashboard\_only\_user角色已经被废弃。如果要查看指定索引的Dashboard，只需要为该索引配置读权限，详细信息，请参见[配置索引只读权限](#section_y3r_vdm_0x4)。
        -   kibana\_dashboard\_only\_user角色与自定义角色配合使用可应用于很多场景。如果您仅需为自定义角色设定**Dashboards only roles**功能，可在**Management**页面的**Kibana**区域，单击**Advanced Settings**，找到**Dashboard**部分，绑定自定义角色（默认是kibana\_dashboard\_only\_user角色）。
-   验证

    通过普通用户登录Kibana控制台，可查看对应索引的Dashboard大盘。

    ![查看Dashboard大盘](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8858997061/p199757.png)


## 配置索引读写和集群只读权限

-   场景描述

    为普通用户授予指定索引的读、写和删除权限，以及集群和Kibana的只读权限。

-   角色配置

    ![索引读写和集群只读权限](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8858997061/p199616.png)

    |权限类型|权限Key|权限Value|描述|
    |----|-----|-------|--|
    |**Cluster privileges**|**cluster**|monitor|集群只读权限。例如查看集群的健康度、状态、热线程、节点信息、阻塞的任务等。|
    |**Index privileges**|indices|heartbeat-\*,library\*|指定索引名称，支持索引全名、别名、通配符及正则表达式。详细信息，请参见[roles-indices-priv](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/defining-roles.html#roles-indices-priv)。|
    |privileges|read|设置索引只读权限。只读权限包括count、explain、get、mget、scripts、search、scroll等操作权限。详细信息，请参见[privileges-list-indices](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-indices)。|
    |create\_index|创建索引权限。如果在创建索引时，定义了索引别名，还需要授予索引manage权限。**说明：** 索引别名需要同时满足indices下定义的匹配规则。 |
    |view\_index\_metadata|索引元数据的只读权限，包括aliases、aliases exists、get index、exists、field mappings、mappings、search shards、type exists、validate、warmers、settings和ilm。|
    |write|对文档执行所有写操作的权限，包括index、update、delete、bulk和更新mapping操作。与create和index权限相比，该权限的覆盖面更大。|
    |monitor|监控所有操作的权限，包括index recovery、segments info、index stats和status。|
    |delete|删除索引文档权限。|
    |delete\_index|删除索引权限。|
    |granted fields|\*|待授权的索引字段，\*表示索引的所有字段。|
    |**Kibana privileges**|privileges|read|为所有空间授予Kibana只读权限。默认为none，表示所有空间无权限访问Kibana。**说明：** Kibana 7.0以下版本仅支持[Base privileges](https://www.elastic.co/guide/en/kibana/6.7/kibana-privileges.html#kibana-privileges)，默认为所有空间授权；7.0及以上版本在Base privileges的基础上，还支持[Feature privileges](https://www.elastic.co/guide/en/kibana/7.10/kibana-privileges.html#kibana-feature-privileges)。即对Kibana特定功能授权，需要指定Kibana空间。 |

-   验证

    通过普通用户登录Kibana控制台，执行如下命令均正常。

    ![验证](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8858997061/p200462.png)


