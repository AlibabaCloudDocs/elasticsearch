# Access to an Alibaba Cloud Elasticsearch cluster from the classic network

This topic provides answers to some frequently asked questions about access to an Alibaba Cloud Elasticsearch cluster from the classic network.

## How do I access an Alibaba Cloud Elasticsearch cluster deployed in a VPC from the classic network?

For network security, your Alibaba Cloud Elasticsearch cluster is deployed in your virtual private cloud \(VPC\). If your business system is deployed in the classic network, you can use the [ClassicLink](/intl.en-US/Elasticsearch Instances Management/FAQ/Access to an Alibaba Cloud Elasticsearch cluster from the classic network.md) feature that is supported by VPC to access your Elasticsearch cluster.

## What is ClassicLink?

The ClassicLink feature is provided by VPC. It provides a network connection that allows you to access your VPC from the classic network.

## What are the limits of ClassicLink?

-   You can connect up to 1,000 classic network-connected ECS instances to a VPC network.
-   A classic network-connected ECS instance can be connected to only one VPC network created under the same account in the same region.

    If you want to connect an ECS instance of Account A to a VPC network that is under Account B, you must first transfer the ECS instance from Account A to Account B.

-   Classic network-connected ECS instances can communicate only with ECS instances in the primary CIDR block of a VPC network. Classic network-connected ECS instances cannot communicate with ECS instances in the secondary CIDR block of the VPC network.
-   To enable the ClassicLink feature for a VPC network, the following conditions must be met.

    |CIDR block of the VPC network|Limit|
    |:----------------------------|:----|
    |172.16.0.0/12|The VPC network does not contain a custom route entry with the following destination CIDR block: 10.0.0.0/8.|
    |10.0.0.0/8|    -   The VPC network does not contain a custom route entry with the following destination CIDR block: 10.0.0.0/8.
    -   Make sure that the CIDR block of the VSwitch that is used to communicate with the classic network-connected ECS instances falls within 10.111.0.0/16. |
    |192.168.0.0/16|    -   The VPC network does not contain a custom route entry with the following destination CIDR block: 10.0.0.0/8.
    -   Add a route to each classic network-connected ECS instance. This route points 192.168.0.0/16 to the Elastic Network Interface \(ENI\) of the ECS instance where the route is added. You can add the route by using the provided script. [Download script](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/58095/cn_zh/1502878832385/route192.zip).

**Note:** Before you run the script, read the readme file in the downloaded package. |


## How do I enable ClassicLink?

1.  Log on to the [VPC console](https://vpcnext.console.aliyun.com/vpc/cn-hangzhou/vpcs).
2.  In the left-side navigation pane, click **VPCs**.
3.  In the top navigation bar, select a region.
4.  Find your desired VPC and click its ID.

    We recommend that you select a VPC that is attached to the Classless Inter-Domain Routing \(CIDR\) block 172.16.0.0/12.

5.  In the upper-right corner of the VPC Details page, click **Enable ClassicLink**.

    **Note:** If ClassicLink is enabled, **Disable ClassicLink** appears.

6.  In the **Enable ClassicLink** dialog box, click **OK**.

    After ClassicLink is enabled, the value of the ClassicLink parameter changes to **Enabled**.


## How do I create a ClassicLink?

Before you create a ClassicLink, you must make sure that the following operations are performed:

-   Read and understand the limits of ClassicLink. For more information, see [What are the limits of ClassicLink?](/intl.en-US/Elasticsearch Instances Management/FAQ/Access to an Alibaba Cloud Elasticsearch cluster from the classic network.md)
-   Enable ClassicLink for the VPC to which you want to establish the ClassicLink. For more information, see [How do I enable ClassicLink?](/intl.en-US/Elasticsearch Instances Management/FAQ/Access to an Alibaba Cloud Elasticsearch cluster from the classic network.md)

1.  Log on to the [ECS console](https://ecs.console.aliyun.com).
2.  In the left-side navigation pane, choose **Instances & Images** \> **Instances**.
3.  In the top navigation bar, select a region.
4.  Find the required ECS instance of the **classic network** type. Then, choose **More** \> **Network and Security Group** \> **Set classic link** in the **Actions** column.
5.  In the dialog box that appears, select your desired VPC, click **OK**, and then click **Go to the instance security group list and add ClassicLink rules**.

    ![Add a ClassicLink rule](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4867819951/p54981.png)

6.  Click **Add ClassicLink Rule**. In the dialog box that appears, configure the following parameters and click **OK**.

    |Parameter|Description|
    |:--------|:----------|
    |**Classic Security Group**|The name of the security group for the classic network.|
    |**Select VPC Security Group**|Select a security group for the VPC.|
    |**Mode**|Select one of the following authorization modes:     -   **Classic <=\> VPC**: This mode allows ECS instances in the classic network and cloud resources in a VPC to access each other. We recommend that you select this mode.
    -   **Classic =\> VPC**: This mode allows ECS instances in the classic network to access cloud resources in a VPC.
    -   **VPC =\> Classic**: This mode allows cloud resources in a VPC to access ECS instances in the classic network. |
    |**Protocol**|Select a communication protocol, such as **Custom TCP**.|
    |**Port Range**|The ports that are used for communication. Specify the ports in the xx/xx format. For example, to specify port 80, enter 80/80.|
    |**Priority**|The priority of the rule. A small value indicates a high priority. For example, if you set this parameter to 1, the rule has the highest priority.|
    |**Description**|The description for the security group.|


## How do I test the connectivity between the classic network and a VPC?

You can perform the test by using the following methods:

-   Go to the [ECS console](https://ecs.console.aliyun.com) and click the ![Column Filters icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0471912261/p249853.png) icon in the upper-right corner of the Instances page. In the Column Filters dialog box, select **Connection Status** and click **OK**. Then, you can view the connection status of the ECS instance.
-   Log on to the ECS instance from which the ClassicLink is established and run the curl command to access your Elasticsearch cluster in the VPC.

    **Note:** If the system displays "curl command not found", run the `yum install curl` command to install cURL on the ECS instance.

    ```
    curl -u <username>:<password> http://<host>:<port>
    ```

    |Parameter|Description|
    |---------|-----------|
    |`<username>`|The account that is used to access your Elasticsearch cluster. We recommend that you do not use the elastic account. **Note:**

    -   If you reset the password of the elastic account when you use the account to access your Elasticsearch cluster, it may require some time for the new password to take effect. During this period of time, you cannot use the account to access your Elasticsearch cluster. Therefore, we recommend that you do not use the elastic account to access your Elasticsearch cluster.
    -   If the version of your Elasticsearch cluster contains "with\_X-Pack", you must specify both the username and password to access the cluster. |
    |`<password>`|The password that is used to access your Elasticsearch cluster. The password is specified when you create the cluster or initialize Kibana.|
    |`<host>`|The **internal endpoint** of your Elasticsearch cluster. You can obtain the internal endpoint from the **Basic Information** page of the cluster.|
    |`<port>`|The port number of your Elasticsearch cluster. In most cases, **9200** is used. You can obtain the port number from the **Basic Information** page of the cluster.|

    Example command:

    ```
    curl -u elastic:es_password http://es-cn-vxxxxxxxxxxxxmedp.elasticsearch.aliyuncs.com:9200
    ```

    If the connection is established, the result shown in the following figure is returned.

    ![Successful response](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1224649951/p58858.png)


