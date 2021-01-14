# Integrate X-Pack with LDAP

This topic describes how to configure Lightweight Directory Access Protocol \(LDAP\) for Alibaba Cloud Elasticsearch. After LDAP is configured, you can use LDAP with assigned roles to access Alibaba Cloud Elasticsearch.

## Precautions

Due to the adjustment made to the Alibaba Cloud Elasticsearch network architecture, clusters created after October 2020 do not support some features. These features include X-Pack Watcher, LDAP authentication, cross-cluster reindexing, cross-cluster searches, and cluster interconnection. The features will be available soon.

## Prerequisites

-   An Alibaba Cloud Elasticsearch cluster is created. In this topic, a V6.7.0 cluster is used.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

-   SNAT entries are created in the NAT gateway.

    The Elasticsearch cluster is deployed in a virtual private cloud \(VPC\). To ensure communication between LDAP and the cluster, you must configure SNAT entries to connect the cluster to the Internet. The procedure for configuring the SNAT entries is the same as that for configuring a NAT gateway for Logstash.

-   The LDAP environment and user data are prepared.

    For more information, see [LDAP official documentation](http://www.openldap.org/doc/admin24/quickstart.html).

    ![LDAP environment and user data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1757359951/p76477.png)


## Background information

Take note of the following items when you configure LDAP:

-   You cannot configure LDAP in Alibaba Cloud Elasticsearch by yourself. If you want to use LDAP to authenticate requests sent to your Alibaba Cloud Elasticsearch cluster, create an on-premises Elasticsearch cluster of the same version as your Alibaba Cloud Elasticsearch cluster. Then, use the on-premises Elasticsearch cluster to run an authentication test. If LDAP runs normally, send the related configurations to Alibaba Cloud Elasticsearch technical engineers to configure LDAP for your Alibaba Cloud Elasticsearch cluster. Otherwise, your online services may be affected. Alibaba Cloud Elasticsearch does not provide a service level agreement \(SLA\) for LDAP authentication.
-   You can configure LDAP only for single-zone clusters.

## Configure LDAP

The following two modes are provided:

-   User search mode
-   Mode where specific templates for distinguished names \(DNs\) of users are configured

The user search mode is more common. In this mode, a specific user with permissions to query the LDAP directory is used to search for the DN of an authenticating user. The search is performed based on the username and LDAP attribute provided by X-Pack. After the DN is found, X-Pack uses the DN and provided password to authenticate the user by attempting to bind the user to the LDAP server. For more information, see [Configure an LDAP realm](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/configuring-ldap-realm.html).

The following configurations specify the mapping that is used by LDAP to manage a DN. You must add the configurations to the YML file of your Elasticsearch cluster.

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

|Parameter|Description|
|---------|-----------|
|type|The type of the realm. This parameter must be set to ldap.|
|url|The URL and port for the LDAP server. ldap indicates that a common connection and port 389 are used. ldaps indicates that an SSL-encrypted connection and port 636 are used.|
|bind\_dn|The DN of the user that is used to bind authenticating users to the LDAP server and perform searches. This parameter is valid only in the user search mode.|
|bind\_password|The password of the user that is used to bind authenticating users to the LDAP server.|
|user\_search.base\_dn|The container DN that is used to search for users.|
|group\_search.base\_dn|The container DN that is used to search for groups to which the user belongs. If this parameter is not configured, Elasticsearch searches for the attribute specified by the user\_group\_attribute parameter to determine group membership.|
|unmapped\_groups\_as\_roles|The default value of this parameter is false. If you set this parameter to true, the names of unmapped LDAP groups are used as role names.|

For more information about the parameters, see [Security settings in Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-settings.html#ref-ldap-settings).

## Map roles to realm accounts

Run the following command to map an administrator role to the lettie\* account:

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

## Verify results

Run the following command and use the authorized lettie account for testing:

```
# curl -XGET -u lettie:<password> http://es-cn-v0h1****.public.elasticsearch.aliyuncs.com:9200/_cat/indices?v
```

![Use the lettie account for testing](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1757359951/p76483.png)

Run the following command and use the unauthorized cent account for testing. The command output indicates that the cent account does not have the required permissions.

![Use the cent account for testing](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1757359951/p76484.png)

