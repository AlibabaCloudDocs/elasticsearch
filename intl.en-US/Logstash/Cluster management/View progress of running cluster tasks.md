# View progress of running cluster tasks

In the task list, you can view the progress of tasks that are running in the cluster, such as cluster creation or restart.

The cluster is in the **Initializing** state.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  Click the ![Tasks icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2187659951/p59966.png) icon in the upper-right corner.

5.  In the **Tasks** dialog box, view the cluster update progress.

6.  Click **Show Details** to view detailed information about the cluster update.

    ![View task progress](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2187659951/p59968.png)

    During the update, you can click **View Logs** to view the operations log of the cluster on the **Logs** page. For more information, see [Query logs](/intl.en-US/Elasticsearch Instances Management/Query logs.md).

    If you want to pause the task, click **Pause**. After the task is paused, you can click **Resume** to resume the task.

    **Note:**

    -   If a cluster has a task paused, it is in the **Paused** state. If your services running on the cluster are affected, resume the task or run the task again. You can resume only cluster configuration upgrade tasks or plug-in management tasks.
    -   After you click **Resume**, Elasticsearch restarts all the nodes in the cluster, which may require a short period of time.

