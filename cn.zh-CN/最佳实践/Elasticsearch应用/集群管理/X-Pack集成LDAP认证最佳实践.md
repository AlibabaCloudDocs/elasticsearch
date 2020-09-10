# X-Pack集成LDAP认证最佳实践

本文介绍如何基于阿里云Elasticsearch配置轻量目录访问协议LDAP（Lightweight Directory Access Protocol）认证，实现通过LDAP用户携带相应角色访问阿里云Elasticsearch。

您已完成以下操作：

-   创建阿里云Elasticsearch实例，本文使用6.7.0版本。

    详情请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   配置NAT网关下的SNAT。

    由于阿里云Elasticsearch服务处于专有网络VPC（Virtual Private Cloud）下，为确保LDAP服务与Elasticsearch之间网络互通，因此需要配置SNAT网关，实现Elasticsearch与公网连通。操作方法与配置Logstash的NAT公网传输类似，详情请参见[配置NAT公网数据传输](/cn.zh-CN/Logstash实例/网络与安全/配置NAT公网数据传输.md)。

-   准备LDAP环境及用户数据。

    详情请参见[LDAP官方文档](http://www.openldap.org/doc/admin24/quickstart.html)。

    ![LDAP环境及用户数据](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7212659951/p76477.png)


## 注意事项

配置LDAP认证时，请注意：

-   目前阿里云Elasticsearch产品侧暂不支持LDAP功能。如果您需要使用LDAP协议对接阿里云Elasticsearch进行认证，需要先参考本文档，在本地搭建对应Elasticsearch版本集群环境进行测试，再将正确的配置提供给阿里云Elasticsearch技术工程师进行配置。如果直接上线，可能会对线上业务产生影响，阿里云Elasticsearch对LDAP认证不提供SLA。
-   目前阿里云Elasticsearch只支持为单可用区实例配置LDAP认证，暂不支持多可用区实例。

## 配置LDAP认证

目前，X-Pack集成LDAP认证支持通过以下两种配置方式：

-   用户搜索模式。
-   带有用户DNs特定模板的模式。

其中，用户搜索模式是最常见的操作方式。在此模式中，具有搜索LDAP目录权限的特定用户，根据X-Pack提供的用户名和LDAP属性，搜索进行身份验证的用户的DN。一旦找到，X-Pack将使用找到的DN和提供的密码，尝试绑定到LDAP服务器来验证用户，详情请参见[Configure an LDAP realm](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/configuring-ldap-realm.html)。

以下为LDAP管理DN的映射方式，需要在Elasticsearch的YML文件中添加如下配置。

```
xpack.security.authc.realms.ldap1.type: ldap
xpack.security.authc.realms.ldap1.order: 2
xpack.security.authc.realms.ldap1.url: "ldap://49.100.XX.XX:389"
xpack.security.authc.realms.ldap1.bind_dn: "cn=Manager, dc=srv, dc=world"
xpack.security.authc.realms.ldap1.bind_password: es_password
xpack.security.authc.realms.ldap1.user_search.base_dn: "ou=People,dc=srv, dc=world"
xpack.security.authc.realms.ldap1.user_search.filter: "(cn={0})"
xpack.security.authc.realms.ldap1.group_search.base_dn: "ou=Group,dc=srv, dc=world"
xpack.security.authc.realms.ldap1.unmapped_groups_as_roles: false
```

|参数|说明|
|--|--|
|`type`|设置域。此处必须设置为`ldap`。|
|`url`|指定LDAP服务器URL及端口。`ldap`协议表示使用普通连接，端口为389；`ldaps`表示使用SSL安全连接，端口为636。|
|`bind_dn`|用于绑定到LDAP并执行搜索的用户的DN，仅适用于用户搜索模式。|
|`bind_password`|用于绑定到LDAP目录的用户的密码。|
|`user_search.base_dn`|用户搜索的容器DN。|
|`group_search.base_dn`|用于搜索用户具有成员资格的容器DN。当此参数不存在时，Elasticsearch将搜索`user_group_attribute`指定的属性，来确定成员身份。|
|`unmapped_groups_as_roles`|默认`false`。如果设置为`true`，则任何未映射的LDAP组的名称都将用作角色名称分配给用户。|

更多参数详情请参见[Security settings in Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-settings.html#ref-ldap-settings)。

## 为域账号信息映射角色

执行如下命令，映射LDAP下的`lettie*`账户为管理员角色。

```
POST _xpack/security/role_mapping/ldap_super_user?pretty
{
  "roles": [ "superuser" ],
  "enabled": true,
  "rules": {
    "any": [
      {
        "field": {
          "username": "/lettie*/"
        }
      }
    ]
  }
}
```

## 结果验证

执行如下命令，使用已授权的`lettie`账户进行测试。

```
# curl -XGET -u lettie:<password> http://es-cn-v0h1****.public.elasticsearch.aliyuncs.com:9200/_cat/indices?v
```

![lettie账户测试](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7212659951/p76483.png)

执行以下命令，使用未授权的`cent`账户测试，返回权限不足。

![cent账户测试](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7212659951/p76484.png)

