# 通过IDaaS SAML单点登录阿里云Elasticsearch

阿里云Elasticsearch支持通过SAML协议单点登录Kibana。本文为您介绍如何使用阿里云云盾应用身份服务IDaaS配置基于SAML协议的阿里云Elasticsearch，从而实现单点登录Kibana。

[SAML](https://baike.baidu.com/item/SAML/1033119?fr=aladdin)协议中定义了三个角色：委托人（通常为一名用户）、身份提供者（IdP），服务提供者（SP）。通常委托人从服务提供者那里请求一项服务，服务提供者请求身份提供者并从那里并获得一个身份断言，服务提供者可以基于这一断言进行[访问控制](https://baike.baidu.com/item/%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)的判断，即决定委托人是否有权执行某些服务。

因为Elasticsearch支持使用基于SAML 2.0协议的Web浏览器[单点登录（SSO）](https://baike.baidu.com/item/%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95)，并且支持使用SAML 2.0 Single Logout配置文件，所以能够通过与阿里云云盾应用身份服务IDaaS联合使用，实现单点登录Kibana。本文以Elasticsearch作为SAML协议中的SP角色，[IDaaS]()作为SAML协议中的IdP角色为例展开介绍。

## 前提条件

-   已创建具有协调节点的阿里云Elasticsearch实例，且该实例开启了**使用HTTPS协议**服务。

    -   创建实例的具体操作，请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/Elasticsearch/管理实例/创建阿里云Elasticsearch实例.md)。
    -   只有配置了协调节点的实例才支持**使用HTTPS协议**服务，请确保实例中已经配置了协调节点。
    本文以阿里云Elasticsearch 7.10版本为例，不同版本的Elasticsearch在单点登录配置上可能存在差异，具体以每个版本对应的[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/index.html)为准。

-   已开通云盾IDaaS服务下的EIAM实例，详情请参见[开通和试用流程]()。
-   目前阿里云Elasticsearch单点登录功能需要通过阿里云技术人员手动配置，所以在配置该功能前，需要您通过工单形式将已测试成功的自身业务环境中的配置信息打包提供给阿里云技术人员，以便帮您完成后端配置。

## 操作流程

1.  [步骤一：IDaas SAML应用侧配置](#section_tfi_d4n_rf6)

    在阿里云应用身份服务（ IDaas）侧添加SAML应用并对目标账户进行应用授权。

2.  [步骤二：创建自定义角色并与IDaaS SAML做映射](#section_aov_cos_a4r)

    通过阿里云Elasticsearch实例对应的Kibana创建自定义角色，并将角色信息映射至IDaas SAML。

3.  [步骤三：修改Elasticsearch和Kibana配置文件](#section_snf_dg6_3ed)

    从已创建的IDaaS应用中导出IDaaS SAML元配置文件，并将对应的信息打包，通过工单形式提供给阿里云技术人员，以便为您完成Elasticsearch和Kibana的YML文件配置。

4.  [步骤四：账号登录](#section_p5n_zfr_urh)

    在Kibana登录界面，通过**Log in with saml/saml1**登录方式进行验证。


## 步骤一：IDaas SAML应用侧配置

1.  IDaas侧添加SAML应用。

    1.  以IT管理员账号登录[云盾IDaaS管理控制台](https://yundun.console.aliyun.com/?p=idaas)。具体操作请参见[IT管理员登录]()。

    2.  在左侧导航栏，单击**EIAM 实例列表**，选择目标实例ID。

    3.  在**概览**页面的左侧导航栏中，选择**应用** \> **添加应用**。

    4.  在**标准协议**页签下，找到**SAML**应用，单击右侧**操作**列下的**添加应用**。

        **说明：** 您可以通过**全部**、 **标准协议**或者**定制模板**页签选择要添加的应用类型。

        ![添加SAML应用](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2162984261/p289586.png)

    5.  如果没有现成的证书可以选择，则在**添加应用（SAML）**面板中，单击**SigningKey**。

        填写以下信息生成一个证书，其中的名称信息最好是和这个应用关联的，方便识别。

        ![添加SigningKey](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289782.png)

        |参数名称|说明|
        |----|--|
        |名称|所添加证书的名称，可以为任意值，但最好和应用相关，以便后续识别。|
        |国家|在下拉列表中选择对应的国家即可。|
        |省份|输入对应的省份。|
        |证书长度|系统支持可选择的长度为1024和2048字节，按需选择即可。|
        |有效期|系统支持可选择的有效期为30天、180天、365天和3年，按需选择即可。|

    6.  单击**提交**。

    7.  无论是选择已有的还是新添加的证书，找到对应的SigningKey，单击右侧**操作**列下的**选择**按钮，配置相关信息（带红色`*`号的为必填项）。

        ![配置IDP认证id及sp认证信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289804.png)

        |参数名称|说明|
        |----|--|
        |应用名称|所添加应用的名称，可以为任意值，但最好和应用相关。|
        |IDP IdentityId|IdP使用的身份标识，和Entity ID元数据文件中的属性相匹配。本文示例为：https://es-cn-n6w24ma4v009j\*\*\*\*.elasticsearch.aliyuncs.com/。|
        |SP Entity ID|Kibana实例的唯一标识符，如果将Kibana添加为身份提供者 \(IdP\)的服务提供商时，需要设置此值，建议您使用Kibana实例的基本URL作为实体ID。本文示例为：https://es-cn-n6w24ma4v009j\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601/。|
        |SP ACS URL（SSO Location）|单点登录地址。本文示例为：https://es-cn-n6w24ma4v009j\*\*\*\*.kibana.elasticsearch.aliyuncs.com:5601/api/security/saml/callback。|
        |NameldFormat|名称标识格式类型。本文示例：urn:oasis:names:tc:SAML:2.0:nameid-format:persistent。|
        |Binding|系统默认使用POST方式。|
        |账户关联方式|支持以下两种方式：        -   账户关联：系统按主子账户对应关系进行手动关联，用户添加后需要管理员审批。
        -   账户映射：系统自动将主账户名称或指定的字段映射为应用的子账户。 |

    8.  单击**提交**。

2.  对目标账户进行应用授权。

    1.  在授权对话框中，单击**立即授权**。

    2.  在**应用授权**页面，搜索应用名称，并选中目标账户，单击**保存**。

        ![授权主体](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2027694261/p290125.png)


**说明：** 应用授权之前，请确保已经同步或创建了应用侧的账号信息，详情请参见[账户]()。

## 步骤二：创建自定义角色并与IDaaS SAML做映射

1.  通过Kibana创建自定义角色。

    1.  登录Kibana控制台。

        登录控制台的具体步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  单击左上角的![导航图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2027694261/p290149.png)图标。

    3.  在左侧导航栏，选择**Management** \> **Stack Management**。

    4.  在**Stack Management**页面的左侧导航栏中，选择**Security** \> **Roles**。

    5.  单击右上角**Create role**，输入相关参数配置。

        ![创建role](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289838.png)

        |参数|说明|
        |--|--|
        |**Role name**|角色名称，可自定义。|
        |**Cluster privileges**|可选。选择此角色可以对集群执行的操作，详情请参见[Security privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#_run_as_privilege)。|
        |**Run As privileges**|可选。选择用户，如果还没有创建用户，可不选，在创建用户时再分配该角色，详情请参见[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)。|
        |**Index privileges**|        -   **Indices**：选择对应的索引模式。例如heartbeat-\*。

**说明：** 如果没有索引模式，请先在**Management**页面，单击**Kibana**中的**Index Pattern**，按照页面提示创建一个索引模式。

        -   **Privileges**：为角色分配索引权限。 |

    6.  单击左下角**Create role**，即可创建对应的自定义角色。

2.  将已创建好的自定义角色与IDaaS SAML做映射。

    1.  单击左上角的![Kibana初始界面](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289847.png)图标，选择**Management** \> **Dev Tools**。

    2.  在**Console**页签下，执行如下示例代码。

        ```
        PUT /_security/role_mapping/idaas-test
        {
          "roles": [ "admin_role" ], 
          "enabled": true,
          "rules": {
            "field": { "realm.name": "saml1" }
          }
        }
        ```

        执行成功后，显示结果如下。

        ![映射执行结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289855.png)

        **说明：** 其他高阶配置，请参见官方文档[Configuring role mappings](https://www.elastic.co/guide/en/elasticsearch/reference/7.0/saml-role-mapping.html#saml-role-mapping)。


## 步骤三：修改Elasticsearch和Kibana配置文件

1.  从已创建的IDaaS应用中导出IDaaS SAML元配置文件。

    1.  登录[云盾IDaaS管理控制台](https://yundun.console.aliyun.com/?p=idaas)。

    2.  在左侧导航栏，选择**应用** \> **应用列表**，单击[步骤一：IDaas SAML应用侧配置](#section_tfi_d4n_rf6)中添加的目标应用对应的**操作**列下的**详情**。

    3.  单击**查看详情**。

    4.  在**应用详情**面板中，单击**导出IDaaS SAML元配置文件**。

        ![导出IDaaS SAML元配置文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289866.png)

2.  将导出的IDaaS SAML元配置文件及下面Elasticsearch和Kibana的YML配置文件打包，通过工单形式提供给阿里云技术人员，由技术人员上传至Elasticsearch的config/saml路径下并完成以下操作。

    分别在Elasticsearch及Kibana对应的YML配置文件中增加SAML信息。

    -   Elasticsearch.yml配置示例如下：

        ```
        #elasticsearch.yml配置
        
        xpack.security.authc.token.enabled: 'true'
        xpack.security.authc.realms.saml.saml1:
          order: 0
          idp.metadata.path: saml/metadata.xml
          idp.entity_id: "https://es-cn-n6w24ma4v009j****.elasticsearch.aliyuncs.com/"
          sp.entity_id: "https://es-cn-n6w24ma4v009j****.kibana.elasticsearch.aliyuncs.com:5601/"
          sp.acs: "https://es-cn-n6w24ma4v009j****.kibana.elasticsearch.aliyuncs.com:5601/api/security/saml/callback"
          attributes.principal: "nameid:persistent"
          attributes.groups: "roles"
        ```

        |参数|说明|
        |--|--|
        |xpack.security.authc.token.enabled|是否开启token服务，取值含义如下：        -   true：开启。
        -   flase：不开启。
**说明：** 配置SAML单点登录条件之一，详情请参见[saml-enable-token](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/saml-guide-authentication.html#saml-enable-token)。 |
        |xpack.security.authc.realms.saml.saml1|定义一个名为saml1的身份认证领域，有关领域说明请参见[realms](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/realms.html#realms)。|
        |order|领域优先级。数值越低，优先级越高。|
        |idp.metadata.path|IdP保存的元数据文件的路径。|
        |idp.entity\_id|IdP使用的身份标识符，和元数据文件中的entity\_id属性相匹配。|
        |sp.entity\_id|Kibana实例唯一标识符，如果将Kibana添加为身份提供者 \(IdP\)的服务提供商时，需要设置此值，建议您使用Kibana的基本URL作为实体ID。|
        |sp.acs|断言消费服务（ACS）端点，一般是Kibana中的URL，用来接受来自IdP的身份验证消息，此ACS端点仅支持SAML HTTP-POST绑定，通常配置为`${kibana-url}/api/security/v1/saml`，其中`${kibana-url}`为Kibana基础URL。**说明：** 在Elasticsearch 7.10版本中使用`/api/security/v1/saml`时，Kibana日志中会产生warn日志：**The "/api/security/v1/saml" URL is deprecated and will stop working in the next major version, please use "/api/security/saml/callback" URL instead.**，说明低版本在陆续弃用`/api/security/v1/saml`，[8.0版本将彻底不支持](https://www.elastic.co/guide/en/kibana/master/breaking-changes-8.0.html)，建议使用`/api/security/saml/callback`代替。 |
        |sp.logout|Kibana接收来自IdP的注销信息的URL。类似`sp.acs`，需设置为：`${kibana-url}/logout`，其中`${kibana-url}`为Kibana的基础URL。|
        |attributes.principal|属性主体，包含用户主体（用户名）的SAML属性的名称。详细信息请参见[SAML realm settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/security-settings.html#ref-saml-settings)。|
        |attributes.groups|属性组，包含用户组的SAML属性的名称。详细信息请参见[SAML realm settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/security-settings.html#ref-saml-settings)。|

        **说明：** 以上配置信息需要与IdP端配置信息一致，请参考IdP配置进行填写。更多配置信息请参见[Configure Elasticsearch for SAML authentication](https://www.elastic.co/guide/en/elasticsearch/reference/7.0/saml-guide-authentication.html)。

    -   kibana配置示例如下：

        ```
        # kibana配置
        
        xpack.security.authc.providers:
          saml.saml1:
            order: 0
            realm: "saml1"
          basic.basic1:
            order: 1
            icon: "logoElasticsearch"
            hint: "Typically for administrators"
        ```

        更多配置信息请参见[Configuring Kibana](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/saml-kibana.html)。

        |配置值|说明|
        |---|--|
        |xpack.security.authc.providers|添加SAML提供的程序，以指示Kibana使用SAML SSO作为身份验证方法。|
        |xpack.security.authc.providers.saml.<provider-name\>.realm|设置您在[elasticsearch realm配置](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/saml-guide-authentication.html#saml-create-realm)的SAML领域名称，示例为：saml1。|

        **说明：** Kibana配置SAML后，仅支持符合SAML身份验证的用户登录Kibana。为了便于在Kibana登录界面支持Basic身份登录（尤其在测试环节，可能需要使用elastic用户名和密码登录集群，创建角色及角色映射），您可以在Kibana的YML文件中指定`basic.basic1`部分配置，该部分配置生效后，将会在Kibana的登录界面出现Basic身份登录入口，更多信息说明请参见[Authentication in kibana](https://www.elastic.co/guide/en/kibana/7.10/kibana-authentication.html)。


## 步骤四：账号登录

1.  登录Kibana控制台。

    登录控制台的具体步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  单击**Log in with saml/saml1**。

    ![选择saml/saml1登录kiaban](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289594.png)

    **说明：** **Log in with Elasticsearch**为Basic身份登录入口，如果登录用户无该需求，则无需在kibana.yml文件中设置Basic身份登录，详情请参见[Authentication in kibana](https://www.elastic.co/guide/en/kibana/7.10/kibana-authentication.html)。

3.  输入[步骤一：IDaas SAML应用侧配置](#section_tfi_d4n_rf6)中授权的账户信息，登录成功后即可进入Kibana的**Home**界面。

    ![登录成功后的界面](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2027694261/p290182.png)


