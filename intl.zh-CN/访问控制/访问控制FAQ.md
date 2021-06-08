---
keyword: [es访问控制常见问题, 购买es找不到vpc, 销毁临时用户创建的es]
---

# 访问控制FAQ

本文介绍阿里云Elasticsearch访问控制相关的常见问题。

-   [为什么使用子账号在阿里云Elasticsearch实例购买页面找不到VPC？](#section_xaw_ax9_ni0)
-   [临时用户创建的集群或数据等，会随着临时用户的销毁而销毁吗？](#section_zi7_l2m_71k)
-   [使用Elasticsearch时报错“子账户无权限，请核对子账号权限”，如何处理？](#section_71e_o7x_dr2)
-   [如何创建一个对Elasticsearch实例中索引等资源的只读用户？](#section_7bw_luq_ljd)
-   [授权的角色用户登录Kibana看不到索引，只有管理员elastic账号能看到，怎么办？](#section_pl3_pa3_8qt)

## 为什么使用子账号在阿里云Elasticsearch实例购买页面找不到VPC？

请参见[查看RAM用户基本信息](/intl.zh-CN/用户管理/基本操作/查看RAM用户基本信息.md)，检查是否已经为对应子账号授予了获取专有网络VPC（Virtual Private Cloud）列表的权限。如果没有授权，请参见[为RAM用户授权](/intl.zh-CN/访问控制/为RAM用户授权.md)，为子账号进行授权。

## 临时用户创建的集群或数据等，会随着临时用户的销毁而销毁吗？

通过临时用户创建的阿里云Elasticsearch实例不会随着临时用户的销毁而销毁，并且临时用户对阿里云Elasticsearch实例的修改也不会随着临时用户的销毁而还原，相当于行使主账号的权限进行操作。

## 使用Elasticsearch时报错“子账户无权限，请核对子账号权限”，如何处理？

对子账号授予Elasticsearch的使用权限，将如下之一权限授权给您的子账号，详情请参见[为RAM用户授权](/intl.zh-CN/访问控制/为RAM用户授权.md)。

-   `AliyunElasticsearchReadOnlyAccess`：只读访问阿里云Elasticsearch或Logstash的权限，可用于只读用户。
-   `AliyunElasticsearchFullAccess`：管理阿里云Elasticsearch或Logstash的权限，可用于管理员。

## 如何创建一个对Elasticsearch实例中索引等资源的只读用户？

需要在Kibana控制台中创建只读权限的角色，并将该角色分配给对应用户，详情请参见[索引操作授权](/intl.zh-CN/访问控制/Kibana角色管理/创建角色.md)。

## 授权的角色用户登录Kibana看不到索引，只有管理员elastic账号能看到，怎么办？

在[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)时，为用户分配kibana\_system权限。

![分配kibana_system权限](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8724309951/p103233.png)

