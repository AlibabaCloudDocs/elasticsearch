---
keyword: [IP address whitelist for access to the Kibana console over the Internet, IP address whitelist for access to the Kibana console over an internal network, IP address whitelist for access to the Kibana console]
---

# Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network

To access the Kibana console over the Internet or an internal network, you must add the IP address of your host to a whitelist of Kibana. This topic describes how to configure an IP address whitelist for access to the Kibana console over the Internet or an internal network.

An Alibaba Cloud Elasticsearch cluster is created. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

## Go to the access configuration page

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane, click **Data Visualization**.

5.  In the **Kibana** section of the page that appears, click **Edit Configuration**.

    You can then view the **Network Access Configuration** section on the **Kibana Configuration** page.

6.  In the **Network Access Configuration** section, perform the following operations:

    ![Configure access over the Internet](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6767819951/p49791.png)

    -   [Configure an IP address whitelist for access to the Kibana console over the Internet](#section_ovn_tjs_bcm)

        Add the public IP address of your host to the IP address whitelist for access to the Kibana console over the Internet. Then, you can use this host to access the Kibana console. By default, **127.0.0.1,::1** is added to the whitelist. This indicates that requests from all IPv4 and IPv6 addresses are denied.

        **Note:** After the IP address whitelist for access to the Kibana console over the Internet is configured, you can use the Kibana console to access only services in virtual private clouds \(VPCs\). You cannot use the Kibana console to access Internet services such as Baidu Maps and AMAP.

    -   [Configure an IP address whitelist for access to the Kibana console over an internal network](#section_ahf_n6m_1o4)

        Add the private IP address of your host to the IP address whitelist for access to the Kibana console over an internal network. Then, you can use this host to access the Kibana console. You can configure the whitelist only after you enable the Private Network Access feature. This feature is disabled by default.


## Configure an IP address whitelist for access to the Kibana console over the Internet

1.  In the **Network Access Configuration** section of the **Kibana Configuration** page, check whether **Public Network Access** is turned on \(indicated by the color green\).

    **Note:**

    -   **Public Network Access** is turned on by default.
    -   If **Public Network Access** is turned off, you cannot log on to the Kibana console over the Internet.
    -   If yes, go to the next step.
    -   If no, click **Public Network Access** to turn it on.
2.  Click **Update** next to **Kibana Whitelist**.

3.  Enter the IP address that you want to add in the text box.

    You can add IP addresses or Classless Inter-Domain Routing \(CIDR\) blocks. Enter IP addresses in the `192.168.0.1` format and CIDR blocks in the `192.168.0.0/24` format. Separate multiple IP addresses and CIDR blocks with commas \(,\). You can enter `127.0.0.1` to deny requests from all IPv4 addresses or enter `0.0.0.0/0` to allow requests from all IPv4 addresses.

    If your Elasticsearch cluster is deployed in the China \(Hangzhou\) region, you can add IPv6 addresses to the whitelist in the `2401:b180:1000:24::5` format or CIDR blocks in the `2401:b180:1000::/48` format. You can enter `::1` to deny requests from all IPv6 addresses or enter `::/0` to allow requests from all IPv6 addresses.

    **Warning:** The default setting `0.0.0.0/0,::/0` indicates that requests from all public IP addresses are allowed, which may cause security risks.

4.  Click **OK**.


## Configure an IP address whitelist for access to the Kibana console over an internal network

1.  In the **Network Access Configuration** section of the **Kibana Configuration** page, check whether **Private Network Access** is turned on \(indicated by the color green\).

    **Note:**

    -   **Private Network Access** is turned off \(indicated by the color gray\) by default.
    -   If **Private Network Access** is turned on, you can log on to the Kibana console over an internal network.
    -   If yes, go to the next step.
    -   If no, click **Private Network Access** to turn it on.
2.  Click **Update** next to **Private Network Whitelist**.

3.  Enter the IP address that you want to add in the text box.

    You can add IP addresses or CIDR blocks. Enter IP addresses in the `192.168.0.1` format and CIDR blocks in the `192.168.0.0/24` format. Separate multiple IP addresses and CIDR blocks with commas \(,\). You can enter `127.0.0.1` to deny requests from all IPv4 addresses or enter `0.0.0.0/0` to allow requests from all IPv4 addresses.

4.  Click **OK**.


