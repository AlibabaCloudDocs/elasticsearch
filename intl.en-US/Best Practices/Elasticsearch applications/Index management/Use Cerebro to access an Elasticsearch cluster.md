---
keyword: use Cerebro to access an Elasticsearch cluster
---

# Use Cerebro to access an Elasticsearch cluster

In addition to Kibana, curl commands, and clients, you can use third-party plug-ins or tools such as Elasticsearch-Head and Cerebro to access an Elasticsearch cluster. The Elasticsearch-Head plug-in is not maintained in versions later than Elasticsearch 5.x. Therefore, we recommend that you use Cerebro to access your Elasticsearch cluster. This topic describes how to use Cerebro to access an Elasticsearch cluster.

-   An Alibaba Cloud Elasticsearch cluster is created.

    For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md).

-   An Alibaba Cloud Elastic Compute Service \(ECS\) instance is created. This instance must reside in the same virtual private cloud \(VPC\) as the Elasticsearch cluster.

    For more information, see [Create an instance by using the provided wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the provided wizard.md). The ECS instance is used to install Cerebro.

    **Note:** If your ECS instance resides in a different VPC from your Elasticsearch cluster, or you want to install Cerebro on an on-premises machine, you can access the Elasticsearch cluster over the Internet. In this case, take note of the following items:

    -   Access over the Internet is less secure than access over an internal network.
    -   Network latency may cause services to be unstable.
    -   You must turn on Public Network Access for your Elasticsearch cluster and configure a whitelist for access to the Elasticsearch cluster over the Internet. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).
-   The [JDK](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html) is installed on the ECS instance. The JDK version must be 1.8 or later.

-   [Cerebro](https://docs.cerebrohq.com/en/articles/3292944-what-is-cerebro) is a third-party tool.
-   You can use Cerebro to access the Elasticsearch cluster over the Internet by using the public endpoint and the related port of this cluster.

1.  Connect to the ECS instance.

    For more information, see [Connect to an ECS instance]().

2.  Download and decompress the [Cerebro](https://github.com/lmenezes/cerebro/releases) installation package.

    -   Run the following command to download the Cerebro installation package:

        ```
        wget https://github.com/lmenezes/cerebro/releases/download/v0.9.0/cerebro-0.9.0.tgz
        ```

    -   Run the following command to decompress the Cerebro installation package:

        ```
        tar -zxvf cerebro-0.9.0.tgz
        ```

3.  Modify the configuration file of Cerebro and associate Cerebro with the Elasticsearch cluster that you want to access.

    1.  Open the application.conf file.

        ```
        vim cerebro-0.9.0/conf/application.conf
        ```

    2.  Configure `hosts` based on the following instructions.

        ![Configure Cerebro](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1363742061/p128887.png)

        **Note:** You can associate Cerebro with multiple Elasticsearch clusters. Multiple clusters are separated with commas \(,\).

        |Parameter|Description|
        |---------|-----------|
        |host|The URL that is used to access the Elasticsearch cluster. Specify the URL in the format of `http://<Internal endpoint of the Elasticsearch cluster>:9200`. You can obtain the internal endpoint from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
        |name|The ID of the Elasticsearch cluster. You can obtain the ID from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
        |username|The username that is used to access the Elasticsearch cluster. Default value: elastic. **Note:** To ensure system security, we recommend that you do not use the elastic username. You can use a custom username instead. Before you use a custom username, you must create a role for it and grant the required permissions to the role. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md). |
        |password|The password that corresponds to the username. The password that corresponds to the elastic username is specified when you create your Elasticsearch cluster. If you forget the password, you can reset it. For more information about the precautions and procedures for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

    3.  Start Cerebro after you save the modifications.

        ```
        cd cerebro-0.9.0
        bin/cerebro
        ```

        After Cerebro is started, the result shown in the following figure is returned.

        ![Cerebro started](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1363742061/p128924.png)

4.  Use Cerebro to access the Elasticsearch cluster.

    1.  Configure a security group for the ECS instance. On the **Inbound** tab, add the IP address of the Elasticsearch cluster that you want to access and set Port Range to 9000.

        For more information, see [Add security group rules](/intl.en-US/Security/Security groups/Add security group rules.md).

    2.  Enter http://<Public IP address of the ECS instance\>:9000 in the address bar of a browser.

    3.  On the logon page of Cerebro, click the ID of the Elasticsearch cluster that you want to access.

        ![Click the cluster name](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1363742061/p128926.png)

    4.  In the Cerebro console, view the status and the number of indexes, shards, and documents of the cluster and perform operations as required.

        ![Cerebro console](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1363742061/p128927.png)

        **Note:** For more information about the usage notes of Cerebro, see [Getting Started with Cerebro](https://docs.cerebrohq.com/en/collections/1978931-getting-started-with-cerebro).


