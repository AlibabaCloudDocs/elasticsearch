# 通过Elastic实现Kubernetes容器全观测

Elastic可观测性是通过Kibana可视化能力，将日志、指标及APM数据结合在一起，实现对容器数据的观测和分析。当您的应用程序以Pods方式部署在Kubernetes中，可以在Kibana中查看Pods生成日志、主机和网络上的事件指标及APM数据，逐步缩小排查范围进行故障排查。本文介绍具体的实现方法。

-   创建阿里云Elasticsearch实例，版本为6.8，并开启白名单和自动创建索引功能。

    具体操作，请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/Elasticsearch/管理实例/创建阿里云Elasticsearch实例.md)、[配置Elasticsearch公网或私网访问白名单](/intl.zh-CN/Elasticsearch/安全配置/配置ES公网或私网访问白名单.md)和[配置YML参数](/intl.zh-CN/Elasticsearch/ES集群配置/配置YML参数.md)。

-   创建ACK集群，并运行Pod服务。本文的测试场景使用的Kubernetes版本为1.18.8-aliyun.1，ECS规格为2C8G。

    具体操作，请参见[创建Kubernetes托管版集群](/intl.zh-CN/Kubernetes集群用户指南/集群/创建集群/创建Kubernetes托管版集群.md)。

-   配置kubectl客户端，可通过kubectl连接Kubernetes集群。

    具体操作，请参见[通过kubectl管理Kubernetes集群](/intl.zh-CN/Kubernetes集群用户指南/集群/连接集群/通过kubectl管理Kubernetes集群.md)。


本文介绍如何使用Elastic实现对Kubernetes容器的全方位检测，具体内容如下：

-   [通过Metricbeat实现指标采集](#section_9rx_i8q_j37)
-   [通过Filebeat实现日志采集](#section_nwq_409_5cs)
-   [通过Elastic APM实现应用程序性能监测](#section_hsx_rlz_x2b)

关于Metricbeat、Filebeat及APM更多的特性说明，请参见[Infrastructure monitoring](https://www.elastic.co/cn/infrastructure-monitoring)、[Log monitoring](https://www.elastic.co/cn/log-monitoring)和[Elastic APM](https://www.elastic.co/cn/apm)。

## 通过Metricbeat实现指标采集

在Kubernetes上部署Metricbeat，有以下两种方式：

-   DaemonSet：保证每个节点运行一个Pod，可以实现对Host指标、System指标、Docker统计信息以及Kubernetes上运行的所有服务指标的采集。
-   Deployment：部署单个Metricbeat实例，该实例用于检索整个集群的唯一指标，如kubernetes event或者kube-state-metrics。

**说明：**

-   本文以同时使用DaemonSet和Deployment的部署方式为例介绍如何部署Metricbeat容器，您也可以仅使用DaemonSet方式进行部署。
-   Metricbeat依赖kube-state-metrics监控，部署前需要确保已完成kube-state-metrics的部署。阿里云ACK容器默认在arms-prom命名空间下部署了kube-state-metrics监控。

1.  通过kubectl访问云容器，下载Metricbeat配置文件。

    ```
    curl -L -O https://raw.githubusercontent.com/elastic/beats/6.8/deploy/kubernetes/metricbeat-kubernetes.yaml
    ```

2.  修改Metricbeat配置文件。

    **说明：**

    官方下载的YML文件中，DaemonSets和Deployments的资源使用extensions/v1beta1，而v1.18及以上版本的Kubernetes，DaemonSets、Deployments和Replicasets资源的extensions/v1beta1 API将被废弃，请使用apps/v1。

    1.  修改kind: Deployment和kind: DaemonSet下的配置信息。

        -   修改环境变量，具体内容如下：

            ```
            env:
            - name: ELASTICSEARCH_HOST
              value: es-cn-nif23p3mo0065****.elasticsearch.aliyuncs.com
            - name: ELASTICSEARCH_PORT
              value: "9200"
            - name: ELASTICSEARCH_USERNAME
              value: elastic
            - name: ELASTICSEARCH_PASSWORD
              value: ****
            - name: KIBANA_HOST
              value: es-cn-nif23p3mo0065****-kibana.internal.elasticsearch.aliyuncs.com
            - name: KIBANA_PORT
              value: "5601"
            ```

            **说明：** 下载的Metricbeat配置文件中默认未定义Kibana相关变量，可以通过容器env传入变量信息。

            |参数|说明|
            |--|--|
            |ELASTICSEARCH\_HOST|阿里云Elasticsearch实例的私网地址。|
            |ELASTICSEARCH\_PORT|阿里云Elasticsearch实例的私网端口。|
            |ELASTICSEARCH\_USERNAME|阿里云Elasticsearch的用户名，默认值elastic。|
            |ELASTICSEARCH\_PASSWORD|elastic用户的密码。|
            |KIBANA\_HOST|Kibana私网地址。|
            |KIBANA\_PORT|Kibana私网端口。|

        -   增加spec.selector配置信息，具体内容如下：

            ```
            ## kind: DaemonSet
            spec:
              selector:
                matchLabels:
                   k8s-app: metricbeat
              template:
                metadata:
                  labels:
                    k8s-app: metricbeat
            
            ## kind: Deployment
            spec:
              selector:
                matchLabels:
                   k8s-app: metricbeat
              template:
                metadata:
                  labels:
                    k8s-app: metricbeat
            ```

    2.  分别在name: metricbeat-daemonset-config和name: metricbeat-deployment-config下，配置Kibana Output信息，调用文件中配置的环境变量。

        ```
        output.elasticsearch:
              hosts: ['${ELASTICSEARCH_HOST:elasticsearch}:${ELASTICSEARCH_PORT:9200}']
              username: ${ELASTICSEARCH_USERNAME}
              password: ${ELASTICSEARCH_PASSWORD}
        setup.kibana:
              host: "https://${KIBANA_HOST}:${KIBANA_PORT}"
        setup.dashboards.enabled: true
        ```

    3.  修改metricbeat-daemonset-modules配置，定义system模块监控的cpu、load、memory、network等系统指标，以及kubernetes模块可以获取的监控指标。

        **说明：** 关于Metricbeat更多的模块配置及指标说明，请参见[System module](https://www.elastic.co/guide/en/beats/metricbeat/6.8/metricbeat-module-system.html)和[Kubernetes module](https://www.elastic.co/guide/en/beats/metricbeat/6.8/metricbeat-module-kubernetes.html)。

        ```
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: metricbeat-daemonset-modules
          namespace: kube-system
          labels:
            k8s-app: metricbeat
        data:
          system.yml: |-
            - module: system
              period: 10s
              metricsets:
                - cpu
                - load
                - memory
                - network
                - process
                - process_summary
                - core
                - diskio
                - socket
              processes: ['.*']
              process.include_top_n:
                by_cpu: 5      # include top 5 processes by CPU
                by_memory: 5   # include top 5 processes by memory
        
            - module: system
              period: 1m
              metricsets:
                - filesystem
                - fsstat
              processors:
              - drop_event.when.regexp:
                  system.filesystem.mount_point: '^/(sys|cgroup|proc|dev|etc|host|lib)($|/)'
          kubernetes.yml: |-
            - module: kubernetes
              metricsets:
                - node
                - system
                - pod
                - container
                - volume
              period: 10s
              host: ${NODE_NAME}
              hosts: ["localhost:10255"]
        ```

    4.  修改metricbeat-deployment-modules配置，获取kube-state-metric监控指标和event服务指标。

        **说明：** Metricbeat服务创建在kube-system的namespace下，kube-state-metrics默认创建在arms-prom的namespace下，由于namespace不同，所以hosts的命名格式为kube-state-metrics.<namespace\>:8080。如果Metricbeat服务与kube-state-metrics模块都创建在同一namespace下，则hosts的命名格式为kube-state-metrics:8080。

        ```
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: metricbeat-deployment-modules
          namespace: kube-system
          labels:
            k8s-app: metricbeat
        data:
          # This module requires `kube-state-metrics` up and running under `kube-system` namespace
          kubernetes.yml: |-
            - module: kubernetes
              metricsets:
                - state_node
                - state_deployment
                - state_replicaset
                - state_pod
                - state_container
              period: 10s
              host: ${NODE_NAME}
              hosts: ["kube-state-metrics.arms-prom:8080"]
            # Uncomment this to get k8s events:
            - module: kubernetes
              metricsets:
                - event
        ```

    5.  配置RBAC权限声明，保证Metricbeat可以获取到Kubernetes集群资源信息。

        ```
        apiVersion: rbac.authorization.k8s.io/v1beta1
        kind: ClusterRoleBinding
        metadata:
          name: metricbeat
        subjects:
        - kind: ServiceAccount
          name: metricbeat
          namespace: kube-system
        roleRef:
          kind: ClusterRole
          name: metricbeat
          apiGroup: rbac.authorization.k8s.io
        ---
        apiVersion: rbac.authorization.k8s.io/v1beta1
        kind: ClusterRole
        metadata:
          name: metricbeat
          labels:
            k8s-app: metricbeat
        rules:
        - apiGroups: [""]
          resources:
          - nodes
          - namespaces
          - events
          - pods
          verbs: ["get", "list", "watch"]
        - apiGroups: ["extensions"]
          resources:
          - replicasets
          verbs: ["get", "list", "watch"]
        - apiGroups: ["apps"]
          resources:
          - statefulsets
          - deployments
          verbs: ["get", "list", "watch"]
        - apiGroups:
          - ""
          resources:
          - nodes/stats
          verbs:
          - get
        ---
        apiVersion: v1
        kind: ServiceAccount
        metadata:
          name: metricbeat
          namespace: kube-system
          labels:
            k8s-app: metricbeat
        ---
        ```

3.  创建资源对象，查看资源状态。

    通过kubectl执行以下命令：

    ```
    kubectl apply -f metricbeat-kubernetes.yaml
    kubectl get pods -n kube-system
    ```

    **说明：** 请确保Pods资源均处于Running状态，否则在Kibana平台有可能会看不到相应数据。

4.  在Kibana查看监测数据。

    1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

        具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  单击左侧菜单**Infrastructure**。

    3.  查看Hosts、Kubernetes Pods对应的Metrics信息。

        -   查看Hosts对应的Metrics信息：单击右上角**Hosts**，在**Map View**页签下，单击指定Host，选择**View metrics**，就可以查看对应的CPU、Load、Memory等指标数据。

            ![Hosts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1425707161/p258785.png)

            ![Host metrics](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2425707161/p258787.png)

        -   查看Kubernetes Pods对应的Metrics信息：单击右上角**Kubernetes**，在**Map View**页签下，单击指定Pod，选择**View metrics**，就可以查看对应的CPU、Memory、Network等指标数据。

            ![Pod metrics](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1224707161/p258780.png)

    4.  查看Kubernetes集群资源总览数据。

        单击左侧菜单**Dashboard**，选择**\[Metricbeat Kubernetes\] Overview**，就可以查看集群资源总览数据。

        ![overview](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2425707161/p258791.png)


## 通过Filebeat实现日志采集

本文示例中，Filebeat容器使用DaemonSet控制器部署，确保集群上每一个节点都有一个正在运行的实例采集数据，并且配置文件中的资源部署在kube-system命名空间下。您如果需要改变，可以手动更改配置文件。

1.  下载Filebeat配置文件。

    通过kubectl访问云容器，下载Filebeat配置文件。

    ```
    curl -L -O https://raw.githubusercontent.com/elastic/beats/6.8/deploy/kubernetes/filebeat-kubernetes.yaml
    ```

2.  修改Filebeat配置文件。

    1.  修改kind: DaemonSet下的环境变量。

        ```
        env:
        - name: ELASTICSEARCH_HOST
          value: es-cn-nif23p3mo0065****.elasticsearch.aliyuncs.com
        - name: ELASTICSEARCH_PORT
          value: "9200"
        - name: ELASTICSEARCH_USERNAME
          value: elastic
        - name: ELASTICSEARCH_PASSWORD
          value: ****
        - name: KIBANA_HOST
          value: es-cn-nif23p3mo0065****-kibana.internal.elasticsearch.aliyuncs.com
        - name: KIBANA_PORT
          value: "5601"
        - name: NODE_NAME
          valueFrom:
             fieldRef:
                fieldPath: spec.nodeName
        ```

        |参数|说明|
        |--|--|
        |ELASTICSEARCH\_HOST|阿里云Elasticsearch实例的私网地址。|
        |ELASTICSEARCH\_PORT|阿里云Elasticsearch实例的私网端口。|
        |ELASTICSEARCH\_USERNAME|阿里云Elasticsearch的用户名，默认值elastic。|
        |ELASTICSEARCH\_PASSWORD|elastic用户的密码。|
        |KIBANA\_HOST|Kibana私网地址。|
        |KIBANA\_PORT|Kibana私网端口。|
        |NODE\_NAME|Kubernetes集群Host。|

    2.  修改name: filebeat-config对应的ConfigMap配置信息，配置Kibana Output信息，调用文件中配置的环境变量。

        ```
        output.elasticsearch:
              hosts: ['${ELASTICSEARCH_HOST:elasticsearch}:${ELASTICSEARCH_PORT:9200}']
              username: ${ELASTICSEARCH_USERNAME}
              password: ${ELASTICSEARCH_PASSWORD}
        setup.kibana:
              host: "https://${KIBANA_HOST}:${KIBANA_PORT}"
        ```

    3.  配置Kubernetes，采集容器日志。

        ```
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: filebeat-inputs
          namespace: kube-system
          labels:
            k8s-app: filebeat
        data:
          kubernetes.yml: |-
            - type: docker
              containers.ids:
              - "*"
              processors:
                - add_kubernetes_metadata:
                    host: ${NODE_NAME}
                    in_cluster: true
        ---
        ```

3.  在Kubernetes中部署Filebeat，并查看资源状态。

    通过kubectl执行以下命令：

    ```
    kubectl apply -f filebeat-kubernetes.yaml
    kubectl get pods -n kube-system
    ```

    **说明：** 请确保Pods资源均处于Running状态，否则在Kibana平台有可能会看不到相应数据。

4.  在Kibana查看实时日志。

    1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

        具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  查看Hosts、Kubernetes Pods对应的日志。

        -   查看Hosts对应的日志信息：单击右上角**Hosts**，在**Map View**页签下，单击指定Host，选择**View logs**，就可以查看Host实时日志。
        -   查看Kubernetes Pods对应的日志信息：单击右上角**Kubernetes**，在**Map View**页签下，单击指定Pod，选择**View logs**，就可以查看Pod实时日志。

            ![view logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0951907161/p258968.png)


## 通过Elastic APM实现应用程序性能监测

Elastic APM是基于Elastic Stack构建的应用程序性能监控系统。它提供实时监控软件服务和应用程序的功能，采集传入请求的响应时间和数据库查询、高速缓存调用及外部HTTP请求等的详细性能信息，帮助您更快速的查明并修复性能问题。Elastic APM还可以自动收集未处理的错误、异常及调用栈，帮助您识别出新错误，并关注对应错误发生的次数。

关于Elastic APM更多的介绍，请参见[Elastic APM Overview](https://www.elastic.co/guide/en/apm/get-started/6.8/overview.html)。

1.  部署APM Server容器。

    本文示例使用Kubernetes部署，通过ConfigMap控制器定义apm-server.yml文件，并初始化Pods启动，通过service实现服务自动发现和负载均衡。

    1.  配置apm-server.yml文件。

        完整的配置文件内容如下：

        ```
        ---
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: apm-deployment-config
          namespace: kube-system
          labels:
            k8s-app: apmserver
        data:
          apm-server.yml: |-
              apm-server.host: "0.0.0.0:8200" 
              output.elasticsearch:
                 hosts: ['${ELASTICSEARCH_HOST:elasticsearch}:${ELASTICSEARCH_PORT:9200}']
                 username: ${ELASTICSEARCH_USERNAME}
                 password: ${ELASTICSEARCH_PASSWORD}
              setup.kibana:
                 host: "https://${KIBANA_HOST}:${KIBANA_PORT}"
        
        ---
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: apmserver
          namespace: kube-system
          labels:
            k8s-app: apmserver
        spec:
          selector:
            matchLabels:
               k8s-app: apmserver
          template:
            metadata:
              labels:
                k8s-app: apmserver
            spec:
              serviceAccountName: apmserver
              hostNetwork: true
              dnsPolicy: ClusterFirstWithHostNet
              containers:
              - name: apmserver
                image: docker.elastic.co/apm/apm-server:6.8.14
                args: [
                  "-c", "/etc/apm-server.yml",
                  "-e",
                ]
                env:
                - name: ELASTICSEARCH_HOST
                  value: es-cn-oew20i5h90006****.elasticsearch.aliyuncs.com
                - name: ELASTICSEARCH_PORT
                  value: "9200"
                - name: ELASTICSEARCH_USERNAME
                  value: elastic
                - name: ELASTICSEARCH_PASSWORD
                  value: ****
                - name: KIBANA_HOST
                  value: es-cn-oew20i5h90006****-kibana.internal.elasticsearch.aliyuncs.com
                - name: KIBANA_PORT
                  value: "5601"
                - name: NODE_NAME
                  valueFrom:
                    fieldRef:
                      fieldPath: spec.nodeName
                securityContext:
                  runAsUser: 0
                resources:
                  limits:
                    memory: 50Mi
                  requests:
                    cpu: 20m
                    memory: 30Mi
                volumeMounts:
                - name: config
                  mountPath: /etc/apm-server.yml
                  readOnly: true
                  subPath: apm-server.yml
              volumes:
              - name: config
                configMap:
                  defaultMode: 0600
                  name: apm-deployment-config
        ---
        apiVersion: v1
        kind: Service
        metadata:
          name: apmserver
          namespace: kube-system
          labels:
            k8s-app: apmserver
        spec:
          clusterIP: None
          ports:
          - name: http-metrics
            port: 8200
            targetPort: 8200
          selector:
            k8s-app: apmserver
        ---
        apiVersion: v1
        kind: ServiceAccount
        metadata:
          name: apmserver
          namespace: kube-system
          labels:
            k8s-app: apmserver
        ---
        ```

        **说明：**

        -   Deployment部署资源中，使用docker.elastic.co/apm/apm-server:6.8.14镜像部署Pods容器，镜像版本需要和阿里云Elasticsearch实例版本一致。
        -   通过service对集群暴露8200服务端口，保证APM Agents可与APM Server通信。
        |参数|说明|
        |--|--|
        |ELASTICSEARCH\_HOST|阿里云Elasticsearch实例的私网地址。|
        |ELASTICSEARCH\_PORT|阿里云Elasticsearch实例的私网端口。|
        |ELASTICSEARCH\_USERNAME|阿里云Elasticsearch的用户名，默认值elastic。|
        |ELASTICSEARCH\_PASSWORD|elastic用户的密码。|
        |KIBANA\_HOST|Kibana私网地址。|
        |KIBANA\_PORT|Kibana私网端口。|
        |NODE\_NAME|Kubernetes集群Host。|

    2.  部署APM Server容器，并查看资源状态。

        通过kubectl执行以下命令：

        ```
        kubectl apply -f apm-server.yml
        kubectl get pods -n kube-system
        ```

        **说明：** 请确保Pods资源均处于Running状态，否则在Kibana平台有可能会看不到相应数据。

2.  配置APM Agents。

    本文示例是通过Spring Boot实现一个简单的Web应用并打包为JAR包，并将JAR包和从Maven Central下载的最新Java Agent上传到服务器。详细信息，请参见[Spring Boot](https://spring.io/guides/gs/spring-boot/)和[Maven Central](https://search.maven.org/search?q=g:co.elastic.apm%20AND%20a:elastic-apm-agent)。

    1.  登录Kubernetes节点，在工作目录中创建Dockerfile文件，文件名为myapply。

        Dockerfile文件内容如下：

        ```
        FROM frolvlad/alpine-oraclejdk8
        MAINTAINER peterwanghao.com
        VOLUME /tmp
        ADD spring-boot-0.0.1-SNAPSHOT.jar spring-boot-0.0.1-SNAPSHOT.jar
        ADD elastic-apm-agent-1.21.0.jar elastic-apm-agent-1.21.0.jar
        EXPOSE 8080
        ENTRYPOINT ["java","-javaagent:/elastic-apm-agent-1.21.0.jar","-Delastic.apm.service_name=my-application","-Delastic.apm.server_url=http://apmserver:8200","-Delastic.apm.application_packages=com.example","-jar","/spring-boot-0.0.1-SNAPSHOT.jar"]
        ```

        ENTRYPOINT定义容器启动时运行的Java命令及参数如下：

        |参数|说明|
        |--|--|
        |-javaagent|指定APM Agent代理JAR包。|
        |-Delastic.apm.service\_name|APM Service Name，允许以下字符：a-z、A-Z、0-9、-、\_及空格。|
        |-Delastic.apm.server\_url|APM Server URL，http://apmserver:8200是在apm-server.yml文件中的service定义。|
        |-Delastic.apm.application\_packages|应用程序的基础软件包。|
        |-jar|指定应用JAR包。|

    2.  通过`docker build`命令和Dockerfile定义的myapply文件构建镜像。

        在当前路径下，执行以下命令：

        ```
        docker build -t myapply .
        ```

    3.  将构建好的镜像加载到其他容器节点。

    4.  配置Pods部署文件，文件名为my-application.yaml。

        文件内容如下：

        ```
        ---
        apiVersion: v1
        kind: Pod
        metadata:
          name: my-apply
          namespace: kube-system
          labels:
            app: my-apply
        spec:
          containers:
            - name: my-apply
              image: myapply:latest
              ports:
                - containerPort: 8080
              imagePullPolicy: Never
        ---
        apiVersion: v1
        kind: Service
        metadata:
          name: my-apply
          namespace: kube-system
          labels:
            app: my-apply
        spec:
          type: NodePort
          ports:
          - name: http-metrics
            port: 8080
            nodePort: 30000
          selector:
            app: my-apply
        
        ---
        ```

        **说明：** image为构建好的镜像文件。

    5.  通过kubectl执行以下命令，部署Pods。

        ```
        kubectl apply -f my-application.yaml
        ```

    6.  待所有Pods资源均处于Running状态后，使用Curl访问主机的30000端口。

        执行命令如下：

        ```
        curl http://10.7.XX.XX:30000
        ```

        **说明：** 10.7.XX.XX为Kubernetes的节点IP地址。

        能够访问对应主机后，APM Agents部署成功。

3.  在Kibana控制台查看APM监控数据。

    1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

        具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  单击左侧菜单**APM**。

    3.  单击目标应用程序，本文示例为my-application，可以看到服务的整体性能数据。

        ![APM](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0951907161/p258970.png)

    4.  单击对应的请求接口，可以看到具体的请求信息。

        ![APM 接口详情](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0951907161/p258973.png)

    5.  查看Hosts、Pods相关日志和指标数据。

        单击**Actions**，选择**Show pod logs**、**Show pod metrics**等，查看对应的日志和指标数据。

        ![Actions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0951907161/p258974.png)

        ![Pod Logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0951907161/p258975.png)


## 常见问题

-   问题：Kubernetes配置文件中resources.requests下资源设置较大，Pods无法启动成功。

    解决方案：Metricbeat、Filebeat、APM配置文件中都需要设置resources.requests，建议根据Kubernetes集群规格适当调整设置的值。

-   问题：在部署Metricbeat、Filebeat、APM容器时，一直报错。报错内容类似：`no matches for kind "DaemonSet" in version "extensions/v1beat1"`。

    解决方案：官方下载的YML文件中，Daemonsets和Deployments的资源使用extensions/v1beta1，而v1.18及以上版本的Kubernetes，Daemonsets、Deployments和Replicasets资源的extensions/v1beta1 API将被废弃，请使用apps/v1。


