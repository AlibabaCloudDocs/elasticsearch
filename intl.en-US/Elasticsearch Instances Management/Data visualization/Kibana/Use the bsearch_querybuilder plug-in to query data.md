---
keyword: [bsearch\_querybuilder plug-in, advanced query]
---

# Use the bsearch\_querybuilder plug-in to query data

bsearch\_querybuilder is also known as an advanced query. It is a frontend plug-in. This plug-in provides a visual interface in which you can create complex queries without the need to write complex domain-specific language \(DSL\) statements.

-   An Alibaba Cloud Elasticsearch V6.3 or V6.7 cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md). This topic uses an Alibaba Cloud Elasticsearch V6.3 cluster as an example.

-   The bsearch\_querybuilder plug-in is installed.

    For more information, see [Install a Kibana plug-in](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Install a Kibana plug-in.md).

-   An index is created, and data is imported into the index.

    For more information, see [Quick start](/intl.en-US/Elasticsearch Instances Management/Quick start.md).


Query DSL is an open-source Java framework that is used to define SQL type-safe queries. It allows you to call API operations to send queries instead of writing statements. Query DSL supports JPA, JDO, SQL, Java Collections, RDF, Lucene, and Hibernate Search.

Elasticsearch provides a complete Query DSL for you based on JSON to define queries. Query DSL provides a number of query expressions. Some queries can wrap other queries, such as boolean queries. Some queries can wrap filters, such as constant\_score queries. Some queries can wrap both other queries and filters, such as filtered queries. You can combine any query expression and filter supported by Elasticsearch to create a complex query and filter the returned result. DSL is a complex language and is hard to master. In most cases, users often make mistakes or spend too much time writing DSL statements. The bsearch\_querybuilder plug-in simplifies the writing of DSL statements and improves efficiency.

![Background information of bsearch_querybuilder](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49225.png)

bsearch\_querybuilder has the following features:

-   Easy to learn: bsearch\_querybuilder is a graphical tool. It allows you to create DSL queries with simple click and drag operations. You can customize search conditions without the need for complex coding, which reduces the cost of learning to write DSL statements. It also helps developers write and verify DSL statements.
-   Easy to use: All queries that you have defined are stored in Kibana. These queries are ready for use at all times.
-   Compact: bsearch\_querybuilder only consumes about 14 MiB of disk space and does not stay resident in the memory. This means that the plug-in does not affect the performance of Kibana and Elasticsearch.
-   Secure and reliable: bsearch\_querybuilder does not rewrite, store, or forward user data. The source code of bsearch\_querybuilder has passed the security auditing of Alibaba Cloud.

**Note:** bsearch\_querybuilder only supports Elasticsearch V6.3 or V6.7 clusters.

## Procedure

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

4.  In the top navigation bar of the **Discover** page, click **Query**.

5.  In the section that appears, select a search condition and a filter, and click **Submit**.

    ![Query result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49357.png)

    -   Click the ![Add a search condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49385.png) button to add a search condition.
    -   Click the ![Add a filter for a search condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49386.png) button to add a filter for the search condition.
    -   Click the ![Delete a search condition or filter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49391.png) button to delete a search condition or filter.
    The bsearch\_querybuilder plug-in allows you to create a variety of queries, such as fuzzy queries, boolean queries, and range queries. Examples:

    -   Fuzzy query

        As shown in the following figure, the **email** condition is added for a fuzzy match. The **email** condition matches all email addresses that contain the **iga** keyword.

        ![Fuzzy query](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49284.png)

        The following figure shows the returned result.

        ![Query result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49285.png)

    -   Boolean query

        As shown in the following figure, the **index** condition is set to **tryme\_book**. An OR condition that contains multiple filters is also added to filter data by **type**. The **type** filters are set to **Undergraduate teaching materials**, **Math**, **Foreign language teaching**, and **Undergraduate textbooks**.

        ![Boolean query ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49286.png)

        The following figure shows the returned result.

        ![Query result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49287.png)

    -   Range query

        Range queries allow you to search data by date. As shown in the following figure, the range condition is used to filter data based on the **utc\_time** field. Only data entries created within the specified time range are returned. The specified time range is `[Current time - 240 days, Current time]`.

        ![Range query ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9767819951/p49288.png)

        The following figure shows the returned result.

        ![Query result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0867819951/p49289.png)


