# 通过ADFS单点登录阿里云Elasticsearch

阿里云Elasticsearch支持通过SAML协议单点登录Kibana。本文为您介绍如何基于SAML协议联合使用阿里云Elasticsearch和ADFS服务，从而实现单点登录Kibana。

[SAML](https://baike.baidu.com/item/SAML/1033119?fr=aladdin)协议中定义了三个角色：委托人（通常为一名用户）、身份提供者（IdP），服务提供者（SP）。通常委托人从服务提供者那里请求一项服务，服务提供者请求身份提供者并从那里并获得一个身份断言，服务提供者可以基于这一断言进行[访问控制](https://baike.baidu.com/item/%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)的判断，即决定委托人是否有权执行某些服务。

[ADFS](https://baike.baidu.com/item/ADFS)（Active Directory Federation Services）是Microsoft's推出的Windows Server活动目录联合服务，是一种能够应用于一次会话过程中多个Web应用用户认证的新技术。阿里云Elasticsearch支持基于[SAML](https://baike.baidu.com/item/SAML/1033119?fr=aladdin) 2.0协议的联合身份验证，所以您能够基于SAML 2.0协议将阿里云Elasticsearch与ADFS联合使用，从而实现单点登录Kibana。本文以Elasticsearch作为SAML协议中的SP角色，[IDaaS]()作为SAML协议中的IdP角色为例展开介绍。

## 前提条件

-   AD域服务以及ADFS角色已配置好。
-   已创建阿里云Elasticsearch实例。

    创建实例的具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。本文以阿里云Elasticsearch 7.10版本为例，不同版本的Elasticsearch在单点登录配置上可能存在差异，具体以每个版本对应的[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/index.html)为准。

-   目前阿里云Elasticsearch单点登录功能需要通过阿里云技术人员手动配置，所以在配置该功能前，需要您通过工单形式将下载的ADFS的元数据文件提供给阿里云技术人员，以便帮您完成后端配置。下载元数据文件的方法如下：

    在浏览器地址栏中输入如下地址`https://<ADFS-server>/federationmetadata/2007-06/federationmetadata.xml`即可下载。其中，<ADFS-server\>是您的ADFS服务器域名或IP地址。


## 操作流程

1.  [步骤一：配置用户和用户组](#section_1fn_ek5_6ce)

    在AD域中，配置用户和用户组。

2.  [步骤二：在ADFS中将Elasticsearch配置为可信SAML SP](#section_tjv_axb_llh)

    在ADFS服务管理器中，SAML SP被称作信赖方，本文以Elasticsearch作为SP角色，所以需要先将其配置为信赖方。

3.  [步骤三：为Elasticsearch配置ADFS转换规则](#section_mzh_8fr_aj5)

    Elasticsearch需要ADFS在SAML断言中提供**NameId**、**grouprole**、**name**属性，ADFS中通过**颁发转换规则**来实现这一功能，所以需要进行相关配置。

4.  [步骤四：创建自定义角色并与ADFS SAML域做映射](#section_eld_yrp_eqe)

    通过阿里云Elasticsearch实例对应的Kibana创建自定义角色，并将角色信息映射至ADFS SAML域。

5.  [步骤五：配置Elasticsearch和Kibana的YML文件](#section_xbb_urg_n6c)

    在浏览器中下载元数据文件，并通过工单形式提供给阿里云技术人员，为您完成Elasticsearch和Kibana的YML文件配置。

6.  [步骤六：登录验证](#section_ak6_h0m_7gu)

    通过访问实例域名，登录Kibana界面，通过**Log in with saml/saml1**登录方式进行验证。


## 步骤一：配置用户和用户组

1.  在服务器仪表板页面，单击右上角**工具**，选择**Active Directory 用户和计算机**。

    ![Active Directory 用户和计算机](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9864125261/p291064.png)

2.  在**Active Directory 用户和计算机**页面，选择**操作** \> **新建** \> **组**。

    ![新建组](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8864125261/p291068.png)

3.  在**新建对象-组**页面，填写组名信息。

4.  在**Active Directory 用户和计算机**页面，选择**操作** \> **新建** \> **用户**。填写用户信息，以英文命名用户名称，保存用户名称。

5.  在**Active Directory 用户和计算机**页面，在**User**文件夹中找到新添加的用户，将用户添加至用户组即可。


## 步骤二：在ADFS中将Elasticsearch配置为可信SAML SP

在ADFS中，SAML SP被称作信赖方。设置Elasticsearch作为ADFS的可信SP的操作步骤如下。本文以Windows Server 2012版本为例。

1.  选择手动添加信赖方信任。

    1.  远程登录Windows Server 2012，进入**ADFS**服务管理器界面。

    2.  在左侧导航栏，选择**信任关系** \> **信赖方信任** \> **添加信赖方（A）...**。

    3.  在**添加信赖方信任向导**页面，完成以下操作。

        1.  单击**启动**，选中**手动输入有关信赖方的数据（T）**。

            ![选择手动输入有关信赖方的数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0291215261/p290649.png)

        2.  单击**下一步**。
        3.  输入信赖方的**显示名称（D）**，建议定义为和这个应用相关联的，以方便后续识别。

            ![显示名称](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0291215261/p290650.png)

        4.  单击**下一步**。
        5.  在**选择配置文件**、**配置证书**向导页中均按照默认配置进行，单击**下一步**。
2.  配置信赖方URL。

    1.  在**配置URL**向导页面，选中**启用对SAML 2.0WebSSO协议的支持（A）**，并添加**信赖方SAML 2.0 SSO服务URL（S）**。

        **说明：**

        -   该URL需要定义为[sp.acs](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/saml-guide-authentication.html)，请参考Elasticsearch对应版本的配置。
        -   一般需要指定为<yourKibana-url\>/api/security/v1/saml，但是Elasticsearch 7.10版本使用此路劲测试时，Kibana日志中会产生warn日志：**The "/api/security/v1/saml" URL is deprecated and will stop working in the next major version, please use "/api/security/saml/callback" URL instead.**，说明低版本在陆续弃用`/api/security/v1/saml`，[8.0版本将彻底不支持](https://www.elastic.co/guide/en/kibana/master/breaking-changes-8.0.html)，建议使用`/api/security/saml/callback`代替。
        ![配置URL](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8864125261/p290957.png)

3.  配置信赖方标识符。

    1.  在**配置标识符**向导页中，输入信赖方信任标识符。

        **说明：** Elastic官方推荐定义为：Kibana域名，请参考[sp.entity\_id参数说明](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/saml-guide-authentication.html)。

    2.  单击**添加**。

        ![添加信赖方信任标志符](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9864125261/p290958.png)

    3.  单击**下一步**，一直按照默认配置进行，直到**完成**。


## 步骤三：为Elasticsearch配置ADFS转换规则

Elasticsearch需要ADFS在SAML断言中提供**NameId**、**grouprole**、**name**属性。ADFS中通过**颁发转换规则**来实现这一功能。本文均为示例值，使用时请根据自身业务按需配置。

**说明：** 颁发转换规则（Issuance Transform Rules）：指如何将一个已知的用户属性，经过转换后，颁发为SAML断言中的属性。由于我们要将用户在AD中的Windows账户名颁发为NameId，因此需要添加一个新的规则。

-   NameId

    配置Active Directory中的Windows账户名为SAML断言中的NameId，其操作步骤如下。

    1.  单击**添加规则**。
    2.  在**声明规则模板（C）**中的下拉列表中，选择**转换传入声明**。
    3.  单击**下一步**。
    4.  使用如下示例配置规则，并单击**完成**。
        -   **声明规则名称**：NameId
        -   **传入声明类型**：Windows账户名
        -   **传出声明类型**：名称ID
        -   **传出名称ID格式**：临时标识符
        -   选中**传递所有声明值。**

            ![配置NameID](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8864125261/p290995.png)

-   grouprole

    通过**以声明方式发送LDAP特性**方式从Active Directory存储中提取LDAP特性，将特性映射到传出声明指定的类型中，其操作步骤如下。

    1.  单击**添加规则**。
    2.  在**声明规则模板（C）**中的下拉列表中，选择**以声明方式发送LDAP特性**。
    3.  单击**下一步**。
    4.  使用如下示例配置规则，并单击**完成**。
        -   **声明规则名称**：grouprole
        -   **特性存储（S）**：Active Directory
        -   **LDAP 特性**：Token-Groups-非限定名称
        -   **传出声明类型**：角色

            ![配置groupid](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9864125261/p290997.png)

-   name

    通过**以声明方式发送LDAP特性**方式从Active Directory存储中提取LDAP特性，将特性映射到传出声明指定的类型中，其操作步骤如下。

    1.  单击**添加规则**。
    2.  在**声明规则模板（C）**中的下拉列表中，选择以声明方式发生LDAP特性**以声明方式发送LDAP特性**。
    3.  单击**下一步**。
    4.  使用如下示例配置规则，并单击**完成**。
        -   **声明规则名称（C）**：name
        -   **特性存储（S）**：Active Directory
        -   **LDAP 特性**：Token-Groups-非限定名称
        -   **传出声明类型**：名称

            ![配置name](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9864125261/p291001.png)


## 步骤四：创建自定义角色并与ADFS SAML域做映射

1.  通过Kibana创建自定义角色。

    1.  登录Kibana控制台。

        登录控制台的具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  单击左上角的![导航图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2027694261/p290149.png)图标。

    3.  在左侧导航栏，选择**Management** \> **Stack Management**。

    4.  在**Stack Management**页面的左侧导航栏中，选择**Security** \> **Roles**。

    5.  单击右上角**Create role**，输入相关参数配置。

        ![创建角色](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9864125261/p291011.png)

        |参数|说明|
        |--|--|
        |**Role name**|角色名称，可自定义。|
        |**Cluster privileges**|可选。选择此角色可以对集群执行的操作，详情请参见[Security privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#_run_as_privilege)。|
        |**Run As privileges**|可选。选择用户，如果还没有创建用户，可不选，在创建用户时再分配该角色，详情请参见[创建用户](/cn.zh-CN/访问控制/Kibana角色管理/创建用户.md)。|
        |**Index privileges**|        -   **Indices**：选择对应的索引模式。例如heartbeat-\*。

**说明：** 如果没有索引模式，请先在**Management**页面，单击**Kibana**中的**Index Pattern**，按照页面提示创建一个索引模式。

        -   **Privileges**：为角色分配索引权限。 |

    6.  单击左下角**Create role**，即可创建对应的自定义角色。

2.  将已创建好的自定义角色与ADFS SAML域做映射。

    1.  单击左上角的![Kibana初始界面](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289847.png)图标，选择**Management** \> **Dev Tools**。

    2.  在**Console**页签下，执行如下示例代码。

        ```
        PUT /_security/role_mapping/saml-kibana
        {
          "roles": [ "admin_role" ],
          "enabled": true,
          "rules": {
            "field": { "realm.name": "saml1" }
          }
        }
        ```

        执行成功后，显示结果如下。

        ![映射执行结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8864125261/p290960.png)

        **说明：** 其他高阶配置，请参见官方文档[Configuring role mappings](https://www.elastic.co/guide/en/elasticsearch/reference/7.0/saml-role-mapping.html#saml-role-mapping)。


## 步骤五：配置Elasticsearch和Kibana的YML文件

将下载的元数据文件，通过工单形式提供给阿里云技术人员，由技术人员完成Elasticsearch和Kibana的YML文件配置。

-   Elasticsearch.yml配置示例如下：

    ```
    #elasticsearch.yml配置
    
    xpack.security.authc.realms.saml.saml1:
      order: 0
      idp.metadata.path: saml/FederationMetadata.xml
      idp.entity_id: "http://zhanglei.****.com/adfs/services/trust"
      sp.entity_id: "https://es-cn-n6w24ma4v009j****.kibana.elasticsearch.aliyuncs.com:5601/"
      sp.acs: "https://es-cn-n6w24ma4v009j****.kibana.elasticsearch.aliyuncs.com:5601/api/security/saml/callback"
      attributes.principal: "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
      attribute_patterns.principal: "^([^@]+)@zhanglei\\.com$"
      attributes.groups: "http://schemas.microsoft.com/ws/2008/06/identity/claims/role"
    ```

    |参数|说明|
    |--|--|
    |xpack.security.authc.token.enabled|是否开启token服务，取值含义如下：    -   true：开启。
    -   flase：不开启。
**说明：** 配置SAML单点登录条件之一，详情请参见[saml-enable-token](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/saml-guide-authentication.html#saml-enable-token)。 |
    |xpack.security.authc.realms.saml.saml1|定义一个名为saml1的身份认证领域，有关领域说明请参见[realms](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/realms.html#realms)。|
    |order|领域优先级。数值越低，优先级越高。|
    |idp.metadata.path|IdP保存的元数据文件的路径。|
    |idp.entity\_id|IdP使用的身份标识符，和元数据文件中的entity\_id属性相匹配。获取方法如下：在浏览器地址栏中，输入`https://<ADFS-server>/federationmetadata/2007-06/federationmetadata.xml`，即可下载ADFS的元数据文件，从而获取对应的信息。其中，<ADFS-server\>是您的ADFS服务器域名或IP地址。

![ADFS元数据文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9864125261/p291060.png) |
    |sp.entity\_id|Kibana实例唯一标识符，如果将Kibana添加为身份提供者 \(IdP\)的服务提供商时，需要设置此值，建议您使用Kibana的基本URL作为实体ID。|
    |sp.acs|断言消费服务（ACS）端点，一般是Kibana中的URL，用来接受来自IdP的身份验证消息，此ACS端点仅支持SAML HTTP-POST绑定，通常配置为`<yourKibana-url>/api/security/v1/saml`，其中`<yourKibana-url>`为Kibana基础URL。**说明：** 在Elasticsearch 7.10版本中使用`/api/security/v1/saml`时，Kibana日志中会产生warn日志：**The "/api/security/v1/saml" URL is deprecated and will stop working in the next major version, please use "/api/security/saml/callback" URL instead.**，说明低版本在陆续弃用`/api/security/v1/saml`，[8.0版本将彻底不支持](https://www.elastic.co/guide/en/kibana/master/breaking-changes-8.0.html)，建议使用`/api/security/saml/callback`代替。 |
    |sp.logout|Kibana接收来自IdP的注销信息的URL。类似`sp.acs`，需设置为：`<yourKibana-url>/logout`，其中`<yourKibana-url>`为Kibana的基础URL。|
    |attributes.principal|属性主体，包含用户主体（用户名）的SAML属性的名称。详细信息请参见[SAML realm settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/security-settings.html#ref-saml-settings)。|
    |attribute\_patterns.principal|一个Java正则表达式，应用于用户的主体属性之前，它与attributes.pattern指定的SAML属性相匹配。属性值必须与模式匹配，并且第一个捕获组的值用作主体。例如，^（\[^@\]+）@example\\\\.com$匹配来自“example.com”域的电子邮件地址，并使用本地部分作为主体。详细信息请参见[SAML realm settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/security-settings.html#ref-saml-settings)。|
    |attributes.groups|属性组，包含用户组的SAML属性的名称。详细信息请参见[SAML realm settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/security-settings.html#ref-saml-settings)。|

    **说明：** 以上配置信息需要与IdP端配置信息一致，请参考IdP配置进行填写。更多配置信息请参见[Configure Elasticsearch for SAML authentication](https://www.elastic.co/guide/en/elasticsearch/reference/7.0/saml-guide-authentication.html)。

-   kibana配置示例如下：

    ```
    # kibana配置
    
    xpack.security.authc.providers:
      saml.saml1:
        order: 0
        realm: "saml1"
    
    ###basic部分按需配置，如果配置，可以使用es用户登录，如果不配置，只能使用saml协议登录
      basic.basic1:
        order: 1
        icon: "logoElasticsearch"
        hint: "Typically for administrators"
    ```

    Basic部分的信息请按需配置，更多详细信息请参见[Token-authentication](https://www.elastic.co/guide/en/kibana/7.10/kibana-authentication.html#token-authentication)。

    |配置值|说明|
    |---|--|
    |xpack.security.authc.providers|添加SAML提供的程序，以指示Kibana使用SAML SSO作为身份验证方法。|
    |xpack.security.authc.providers.saml.<provider-name\>.realm|设置您在[elasticsearch realm配置](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/saml-guide-authentication.html#saml-create-realm)的SAML领域名称，示例为：saml1。|

    **说明：** Kibana配置SAML后，仅支持符合SAML身份验证的用户登录Kibana。为了便于在Kibana登录界面支持Basic身份登录（尤其在测试环节，可能需要使用elastic用户名和密码登录集群，创建角色及角色映射），您可以在Kibana的YML文件中指定`basic.basic1`部分配置，该部分配置生效后，将会在Kibana的登录界面出现Basic身份登录入口，更多信息说明请参见[Authentication in kibana](https://www.elastic.co/guide/en/kibana/7.10/kibana-authentication.html)。


## 步骤六：登录验证

1.  通过访问实例域名，登录Kibana控制台。

    示例为：`https://<yourInstanceId>.kibana.elasticsearch.aliyuncs.com:5601/`。

2.  单击**Log in with saml/saml1**。

    ![选择saml/saml1登录kiaban](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3162984261/p289594.png)

    **说明：** **Log in with Elasticsearch**为Basic身份登录入口，如果登录用户无该需求，则无需在kibana.yml文件中设置Basic身份登录，详情请参见[Authentication in kibana](https://www.elastic.co/guide/en/kibana/7.10/kibana-authentication.html)。

3.  输入AD域中存储的用户名和密码，登录成功后即可进入Kibana的**Home**界面。

    ![登录成功后的界面](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2027694261/p290182.png)


