# Installation errors of a custom plug-in

This topic describes how to troubleshoot problems that occur when you install a custom plug-in. These problems include console-reported errors, modification suspension, and verification failures.

## Problem description

When you [upload and install a custom plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Upload and install a custom plug-in.md), problems occur, such as console-reported errors, modification suspension, and verification failures.

## Common solutions

Before you upload and install a custom plug-in, you must put the plug-in in the plugins folder under the installation path of a self-managed Elasticsearch cluster. Then, restart the cluster to load the plug-in. After the cluster is restarted, run the `GET /_cat/plugins?v` command to check whether the plug-in is installed. When you install the plug-in, take note of the following items:

-   A plug-in that has the same name as a built-in plug-in provided by Alibaba Cloud Elasticsearch cannot be uploaded.

    If you want to upload a plug-in that has the same name as a built-in plug-in provided by Alibaba Cloud Elasticsearch, you must change the name of the plug-in first. When you test the installation of a plug-in, such as pinyin analysis or IK analysis, on a self-managed Elasticsearch cluster, first delete the native plug-in that corresponds to the plug-in. Then, change the name of the plug-in that you want to install, and install it by running the following installation command for a native plug-in. If the plug-in is installed, you can upload and install the plug-in to your Alibaba Cloud Elasticsearch cluster.

    ```
    ./bin/elasticsearch-plugin install file:///path-to-your-plugins.zip
    ```

-   Custom plug-ins can be uploaded and installed to an Alibaba Cloud Elasticsearch cluster only after they are tested on a self-managed Elasticsearch cluster. These custom plug-ins are installed to the cluster by running the following installation command for a native plug-in:

    ```
    ./bin/elasticsearch-plugin install file:///path-to-your-plugins.zip
    ```

-   Rolling updates are unavailable for custom plug-ins.
-   If the security policy of a plug-in defines the permissions on addition, deletion, modification, and query, you must comment out these permissions.

    For example, you can view the security policy of the hanIp plug-in in its configuration file. For this plug-in, you must comment out the following configuration in the plugin-security.policy file:

    ```
    permission java.io.FilePermission "<>", "read,write,delete";
    ```

-   Custom plug-ins cannot be installed for Logstash and Kibana. If you want to install a custom plug-in for Logstash or Kibana, you must first install the plug-in on a self-managed Elasticsearch cluster by running the installation command for a native plug-in. If the plug-in is installed, [submit a ticket](https://selfservice.console.aliyun.com/ticket/category/elasticsearch/today) to the technical support engineer of Alibaba Cloud Elasticsearch.

    **Note:** After a valid plug-in is installed, you can query the plug-in logs on the Cluster Log tab of the Elasticsearch console.


## Console-reported errors

-   Cause

    The plug-in that you upload is invalid.

-   Solution

    Modify the information of the plug-in. For more information, see [Common solutions](#section_rs1_9x8_30e). For example, you can rename the plug-in or modify the configuration file of the plug-in.


## Modification suspension

Modification suspension is caused by two reasons. You must perform the following steps to troubleshoot it:

1.  Check the size of the plug-in. Make sure that the size is less than 50 MB.

    If the size of the plug-in is greater than or equal to 50 MB, the loading of the plug-in during the installation is slow. In this case, you must terminate the loading and delete the plug-in. Then, modify the configuration of the plug-in. For example, you can delete some tokens provided by the plug-in. Make sure that the size of the plug-in is less than 50 MB before you upload and install the plug-in again.

2.  Check whether the system is writing data to data nodes.
    -   Yes: Wait for a few minutes. The installation process is slow because the installation is performed during peak hours.
    -   No: Terminate the installation. Then, delete the plug-in and test its availability again.

## Verification failures

Verification failures are caused by various reasons. For example, the version of the plug-in is not compatible with your Alibaba Cloud Elasticsearch cluster, the plugin-descriptor.properties file contains invalid parameter settings, the plug-in is not packaged, or the installation path of the plug-in is invalid. The plug-in is usually packaged in a recursive manner. You can run the zip -r command to package the plug-in. The preceding problems may occur during the installation of open source plug-ins. You need to troubleshoot them on your own.

**Note:** For some plug-ins such as Jieba, if the plug-in version is not compatible with your Alibaba Cloud Elasticsearch cluster, you must modify the version. Then, package the plug-in again, move the obtained package to the plugins directory, and run the `./bin/elasticsearch-plugins install` command to install the plug-in to a self-managed Elasticsearch cluster. If the plug-in fails to be installed, test the availability of the plug-in again based on the error message that appears. The plug-in can be uploaded and installed to your Alibaba Cloud Elasticsearch cluster only after it passes the availability test.

