---
keyword: [bsearch\_label plug-in, data labeling]
---

# Use the bsearch\_label plug-in to label data

bsearch\_label is a frontend data labeling plug-in. It supports visualized data labeling. This way, you do not need to write complex domain-specific language \(DSL\) statements.

-   An Alibaba Cloud Elasticsearch V6.3 or V6.7 cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md). This topic uses an Elasticsearch V6.3 cluster as an example.

-   The bsearch\_label plug-in is installed.

    For more information, see [Install a Kibana plug-in](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Install a Kibana plug-in.md).

-   An index is created, and data is added to the index.

    For more information, see [Quick start](/intl.en-US/Elasticsearch Instances Management/Quick start.md).

-   The language of the Kibana console is English. The default language is English. If the language is not English, change the language.

    For more information, see [Configure the language of the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure the language of the Kibana console.md).


In most cases, when you analyze data, you may want to use query conditions to filter the data in addition to viewing the data and use tags to classify fields. This procedure is known as data labeling. After you add tags to data, you can use the tags to aggregate and classify the data, perform statistical analysis, and filter data by tag. The labeled data can be used in subsequent procedures.

1.  Log on to the Kibana console of your Alibaba Cloud Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Management** and follow these steps to create an index pattern:

    **Note:** If you have created an index pattern, skip this step.

    1.  In the **Kibana** section of the **Management** page, click **Index Patterns**.

    2.  In the **Create index pattern** section, enter an index pattern name \(the name of the index that you want to query\).

    3.  Click **Next step**.

        ![Create an index pattern](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1867819951/p94699.png)

    4.  Click **Create index pattern**.

3.  In the left-side navigation pane, click **Discover**.

4.  In the top navigation bar of the **Discover** page, click **Label**.

5.  Use one of the following methods to label the data.

    -   Add a tag to an existing field.

        ![1 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1867819951/p55145.png)

        1.  As shown in the preceding example, find the record of user zhangsan.
        2.  Select the **age** field and add tag **18** to this filed.
        3.  Click **make tag**.
        4.  Click the **history** switch to view detailed labeling history.

            ![View labeling history](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1867819951/p55146.png)

    -   Add a tag to a new field.

        ![3 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1867819951/p55149.png)

        1.  As shown in the preceding example, find the record of user zhangsan.
        2.  Select **custom marking field**.
        3.  Add the **tag** field and add the **teenager** tag to this field.
        4.  Click **make tag**.
        5.  View the labeling result.

            ![View the labeling result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1867819951/p55152.png)


