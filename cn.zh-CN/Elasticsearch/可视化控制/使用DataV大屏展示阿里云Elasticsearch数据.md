---
keyword: [使用DataV访问阿里云ES服务, DataV大屏展示ES数据]
---

# 使用DataV大屏展示阿里云Elasticsearch数据

通过在DataV中添加阿里云Elasticsearch数据源，您可以使用DataV访问阿里云Elasticsearch服务，完成数据的查询与展示。本文介绍如何使用DataV大屏展示阿里云Elasticsearch数据。

您已完成以下操作：

-   创建阿里云Elasticsearch实例。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

-   开通DataV服务，且版本为企业版或以上版本。

    具体操作，请参见[开通DataV服务](/cn.zh-CN/快速入门/开通DataV服务.md)。

-   准备待展示的索引数据。

    具体操作，请参见[快速开始](/cn.zh-CN/Elasticsearch/快速开始.md)。

    本文使用如下命令创建索引和添加数据：

    -   创建索引

        ```
        PUT /my_index
        {
            "settings" : {
              "index" : {
                "number_of_shards" : "5",
                "number_of_replicas" : "1"
              }
            },
            "mappings" : {
                "my_type" : {
                    "properties" : {
                      "post_date": {          
                           "type": "date"       
                       },
                      "tags": {
                           "type": "keyword"
                       },
                        "title" : {
                            "type" : "text"         
                        }
                    }
                }
            }
        }
        ```

    -   添加数据

        ```
        PUT /my_index/my_type/1?pretty
        {
          "title": "One",
          "tags": ["ruby"],
          "post_date":"2009-11-15T13:00:00"
        }
        PUT /my_index/my_type/2?pretty
        {
          "title": "Two",
          "tags": ["ruby"],
          "post_date":"2009-11-15T14:00:00"
        }
        PUT /my_index/my_type/3?pretty
        {
          "title": "Three",
          "tags": ["ruby"],
          "post_date":"2009-11-15T15:00:00"
        }
        ```


## 在DataV中添加Elasticsearch数据源

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  进入目标实例。

    1.  在顶部菜单栏处，选择资源组和地域。

    2.  在左侧导航栏单击**Elasticsearch实例**，然后在**Elasticsearch实例**中单击目标实例ID。

4.  在左侧导航栏，单击**可视化控制**。

5.  在**DataV**区域中，单击**进入控制台**。

6.  在[DataV控制台](https://datav.aliyun.com/)中，添加Elasticsearch数据源。

    **说明：** DataV企业版及以上版本才支持添加Elasticsearch数据源。

    1.  进入**我的数据**页面，单击**添加数据**。

    2.  从**添加数据**对话框的**类型**列表中，选择**Elastic Search**。

    3.  单击**使用前请授权DataV访问**。

        ![添加数据对话框](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1156359951/p61576.png)

    4.  在**云资源访问授权**页面，单击**同意授权**。

    5.  返回DataV控制台，单击**我的数据**。

    6.  单击**添加数据**。

    7.  从**添加数据**对话框的**类型**列表中，选择**Elastic Search**，并填写阿里云Elasticsearch实例信息。

        |参数|说明|
        |--|--|
        |**自定义数据源名称**|数据源的显示名称，可自定义。|
        |**Region**|实例的地域。|
        |**实例ID**|实例ID，可在实例的基本信息页面获取。详细信息，请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。 授权DataV访问阿里云Elasticsearch服务后，单击**获取实例列表**，可在右侧下拉列表选择某一实例。 |
        |**密码**|实例的访问密码。|

    8.  单击**确定**。

        确定后系统会自动进行测试连接，测试连接成功后即可完成数据源的添加。


## 使用Elasticsearch数据源

在使用阿里云Elasticsearch数据源之前，需要先[在DataV中添加Elasticsearch数据源](#section_q3p_f9l_mwk)。

![已添加的Elasticsearch数据源](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1156359951/p61586.png)

1.  进入[DataV控制台](https://datav.aliyun.com/)。

2.  在**我的可视化**页面，移动鼠标移至您的大屏项目上，单击**编辑**。

    **说明：** 如果还没有大屏项目，请先创建一个大屏项目并添加组件。具体操作，请参见DataV官方文档的[快速入门](/cn.zh-CN/快速入门/制作可视化应用（空白画布篇）/创建可视化应用.md)章节。

3.  在大屏编辑页面的画布中，单击选择某一组件。

    本文以双十一轮播列表柱状图组件为例。

4.  单击**数据**页签，再单击**配置数据源**。

5.  在**设置数据源**页面，选择**数据源类型**为**Elastic Search**，**已有数据源**为您已经添加的阿里云Elasticsearch数据源。

6.  在**index**输入框中填写查询索引。

    查询索引通常为一个字符串，本文使用my\_index索引。

7.  在**Query**输入框中填写查询体。

    查询体通常为一个JSON对象，默认是\{\} 。

8.  启用并配置数据过滤器。

    ![配置Elasticsearch数据源](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1156359951/p61585.png)

    本文使用的过滤器脚本如下，具体配置方法请参见[组件过滤器使用介绍](/cn.zh-CN/组件管理/组件数据过滤器使用说明/使用示例.md)。

    ```
    return data.hits.hits.map(item => {
      return {
        value: item._id,
        content: item._source.title
      };
    });
    ```

9.  在数据过滤器脚本编辑区域，单击空白处，查看过滤器运行结果。

    ![过滤器运行结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2156359951/p95220.png)


预览并发布大屏，展示对应Elasticsearch实例的索引数据。具体操作，请参见[发布PC端可视化应用](/cn.zh-CN/可视化应用管理/发布PC端可视化应用.md)。

