---
keyword: log on to the Kibana console
---

# Log on to the Kibana console

If you purchase an Alibaba Cloud Elasticsearch cluster, Alibaba Cloud provides a free Kibana node with one vCPU and 2 GiB of memory. You can also purchase a Kibana node with higher specifications. You can use the Kibana console to perform operations such as data queries and data visualization. This topic describes how to log on to the Kibana console.

-   An Alibaba Cloud Elasticsearch cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   The Public Network Access or Private Network Access feature is enabled for the Kibana console. By default, the Public Network Access feature is enabled.

    For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).

-   The language of the Kibana console is English. The default language is English. If the language is not English, change the language.

    For more information, see [Configure the language of the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure the language of the Kibana console.md).


The Kibana console provided by Alibaba Cloud Elasticsearch allows you to expand your business. The Kibana console is seamlessly integrated into Elasticsearch. It allows you to view the status of your Elasticsearch cluster in real time and manage the cluster.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane, click **Data Visualization**.

5.  In the **Kibana** section, click **Access over an Internal Network** or **Access over the Internet**.

    -   **Access over an Internal Network**: The Access over an Internal Network entry is displayed only after you enable the **Private Network Access** feature for Kibana. This feature is disabled by default. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).
    -   **Access over the Internet**: The Access over the Internet entry is displayed only after you enable the **Public Network Access** feature for Kibana. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).

        **Note:** If this is the first time you are logging on to the Kibana console from the **Access over the Internet** entry, and you have not modified the access configuration, the system prompts you to modify the configuration. You can click **Edit Configuration** to go to the **Kibana Configuration** page and modify the configuration of access over the Internet. After you modify the configuration, switch back to the Data Visualization page and click **Access over the Internet** again. Then, the Kibana logon page appears.

6.  On the Kibana logon page, enter the username and password and click Log in.

    -   Username: The default username is elastic. You can also customize the username. For more information, see [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md).
    -   Password: the password of the elastic user. The password of the elastic user is specified when you create the Elasticsearch cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).

After you log on to the Kibana console, you can perform the required operations. For example, you can query data or create dashboards to present data. For more information, see [Kibana Guide](https://www.elastic.co/guide/en/kibana/current/index.html).

