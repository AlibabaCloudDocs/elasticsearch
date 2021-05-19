---
keyword: [Auditbeat, use Auditbeat to collect data from the audit framework, use Auditbeat to monitor system file changes]
---

# Use Auditbeat to collect system audit data and monitor file changes

This topic describes how to use Alibaba Cloud Auditbeat to collect data from the Linux audit framework, monitor system file changes, and generate visual charts.

Auditbeat is a lightweight shipper that collects audit logs from the Linux audit framework and monitors file changes. For example, you can use Auditbeat to collect audit events from the Linux audit framework and audit the events in a centralized manner. You can also use Auditbeat to detect changes to critical files, such as binary files and configuration files, and identify potential security policy violations. Then, Auditbeat can generate standard structured data for analytics. Auditbeat can also be seamlessly integrated with Logstash, Elasticsearch, and Kibana.

Auditbeat supports the following modules:

-   Auditd

    The auditd module receives audit events from the Linux audit framework. The framework is a part of the Linux kernel. This module establishes a subscription to the kernel to receive events when they occur. For more information, see [open source Auditbeat documentation](https://www.elastic.co/guide/en/beats/auditbeat/6.8/auditbeat-module-auditd.html).

    **Note:** If you run Auditbeat when the auditd module is enabled, other monitoring tools may affect Auditbeat. For example, if the auditd process is also registered to receive data from the Linux audit framework, Auditbeat may encounter an error. In this case, you can run the `service auditd stop` command to stop the process.

-   File integrity

    The file integrity module monitors the changes to files in a specific directory in real time. To use this module in Linux, make sure that your Linux kernel supports inotify, which is installed for a Linux kernel of 2.6.13 or later. For more information, see [open source Auditbeat documentation](https://www.elastic.co/guide/en/beats/auditbeat/6.8/auditbeat-module-file_integrity.html).

    **Note:** Open source Auditbeat also contains an experimental module named system. The system module may be deleted or changed in a later Auditbeat version. Therefore, we recommend that you do not use this module. For more information, see [Modules](https://www.elastic.co/guide/en/beats/auditbeat/6.8/auditbeat-modules.html).


## Prerequisites

You have completed the following operations:

-   Create an Alibaba Cloud Elasticsearch cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   Enable the **Auto Indexing** feature for the Elasticsearch cluster.

    For security purposes, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. However, Beats depends on this feature. If you select **Elasticsearch** for **Output** when you create a shipper, you must enable the **Auto Indexing** feature. For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).

-   Create an Alibaba Cloud Elastic Compute Service \(ECS\) instance in the same virtual private cloud \(VPC\) as the Elasticsearch cluster.

    For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

    **Note:** Beats supports only Aliyun Linux, Red Hat Linux, and CentOS.

-   Install Cloud Assistant and Docker on the ECS instance.

    For more information, see [Install the Cloud Assistant client](/intl.en-US/Deployment & Maintenance/Cloud assistant/Configure the Cloud Assistant client/Install the Cloud Assistant client.md) and [Deploy and use Docker on Alibaba Cloud Linux 2 instances](/intl.en-US/Tutorials/Build an application/Deploy and use Docker/Deploy and use Docker on Alibaba Cloud Linux 2 instances.md).


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the **Create Shipper** section of the page that appears, click **Auditbeat**.

3.  Install and configure the shipper.

    For more information, see [Collect the logs of an ECS instance](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md) and [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md). The following figure shows the configurations that are used in this topic.

    ![Configure the Auditbeat shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8557359951/p86994.png)

    **Note:**

    -   If you select **Enable Kibana Monitoring**, the system enables Auditbeat service monitoring in the Kibana console.
    -   If you select **Enable Kibana Dashboard**, the system generates charts in the Kibana console. You do not need to configure the YML file. Alibaba Cloud Kibana is configured in a VPC. You must enable the VPC access feature for Kibana on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).
    In this topic, default configurations in the **auditbeat.yml** file are used. No modifications are required for this file. The following configurations are used for related modules:

    -   Auditd

        ```
        - module: auditd
          # Load audit rules from separate files. Same format as audit.rules(7).
          audit_rule_files: [ '${path.config}/audit.rules.d/*.conf' ]
          audit_rules: |
        ```

        -   `audit_rule_files`: specifies the files from which audit rules are loaded. Wildcards are supported. By default, files for 32-bit and 64-bit operating systems are provided. You can select suitable files based on your operating system.
        -   `audit_rules`: specifies audit rules. You can connect to your ECS instance and run the `./auditbeat show auditd-rules` command to view default audit rules.

            ```
            -a never,exit -S all -F pid=26253
            -a always,exit -F arch=b32 -S all -F key=32bit-abi
            -a always,exit -F arch=b64 -S execve,execveat -F key=exec
            -a always,exit -F arch=b64 -S connect,accept,bind -F key=external-access
            -w /etc/group -p wa -k identity
            -w /etc/passwd -p wa -k identity
            -w /etc/gshadow -p wa -k identity
            -a always,exit -F arch=b64 -S open,truncate,ftruncate,create,openat,open_by_handle_at -F exit=-EACCES -F key=access
            -a always,exit -F arch=b64 -S open,truncate,ftruncate,create,openat,open_by_handle_at -F exit=-EPERM -F key=access
            ```

            **Note:** In most cases, the default audit rules can meet your needs. If you want to use custom audit rules, modify the audit rule files in the audit.rules.d directory.

    -   File integrity

        ```
        - module: file_integrity
          paths:
          - /bin
          - /usr/bin
          - /sbin
          - /usr/sbin
          - /etc
        ```

        `paths`: specifies the paths that store the files you want to monitor. Default paths include `/bin`, `/user/bin`, `/sbin`, `/usr/sbin`, and `/etc`.

4.  Select the ECS instance on which you want to install the shipper.

    ![Select the ECS instance on which you want to install a shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82419.png)

5.  Enable the shipper and check whether the shipper installation succeeds.

    1.  Click **Enable**.

        Then, the **Enable Shipper** message appears.

    2.  Click **Back to Beats Shippers**. In the **Manage Shippers** section of the **Beats Data Shippers** page, view the installed shipper.

    3.  After the **state** of the shipper changes to **Enabled 1/1**, click **View Instances** in the **Actions** column.

    4.  In the **View Instances** pane, check whether the shipper installation on the ECS instance succeeds. If the value of **Installed Shippers** is **Heartbeat Normal**, the shipper installation succeeds.

        ![Auditbeat installation succeeded](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8557359951/p87000.png)


## View the collected data

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Discover**. On the page that appears, select **auditbeat-\*** from the drop-down list in the upper-left corner and specify a period in the upper-right corner. Then, view the data collected by Auditbeat within the specified period.

    ![View the data collected by Auditbeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8557359951/p87004.png)

3.  In the left-side navigation pane, click **Dashboard**.

4.  In the **Dashboards** section of the page that appears, click **\[Auditbeat File Integrity\] Overview**. On the page that appears, select a period. Then, view the changes to monitored files within the specified period.

    ![File changes detected by Auditbeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8557359951/p87008.png)


