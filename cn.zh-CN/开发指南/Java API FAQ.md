# Java API FAQ

本文介绍阿里云Elasticsearch在Java API方面的常见问题。

-   [使用Transport Client访问阿里云Elasticsearch，其中cluster.name怎么获取？](#section_dbl_l63_3we)
-   [使用Transport Client连接阿里云Elasticsearch时，报错NoNodeAvailableException如何处理？](#section_1zl_g2x_4b0)

## 使用Transport Client访问阿里云Elasticsearch，其中cluster.name怎么获取？

cluster.name就是实例ID，可在实例的基本信息页面获取，详情请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。

## 使用Transport Client连接阿里云Elasticsearch时，报错NoNodeAvailableException如何处理？

使用5.5或5.6版本的Transport Client与阿里云Elasticsearch建立连接时会提示NoNodeAvailableException的错误，推荐您使用5.3.3版本。使用Transport Client，需要购买5.5或5.6版本的阿里云Elasticsearch实例（6.x及以上版本不支持），并且需要在代码中将`client.transport.sniff`设置为false，详情请参见[Transport Client（5.x）](/cn.zh-CN/开发指南/Java API/Transport Client（5.x）.md)。

**说明：** Elasticsearch 7.0中已经弃用Transport Client，因此在实际开发中建议您使用Java REST Client，详情请参见[High Level REST Client（6.3.x）](/cn.zh-CN/开发指南/Java API/High Level REST Client（6.3.x）.md)、[High Level REST Client（6.7.x）](/cn.zh-CN/开发指南/Java API/High Level REST Client（6.7.x）.md)、[Low Level REST Client \(5.x\)](/cn.zh-CN/开发指南/Java API/Low Level REST Client (5.x).md)。

