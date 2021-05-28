# 基于Terraform管理阿里云ES最佳实践

通过Terraform，您可以使用代码配置实现物理机等资源的分配。也就是说通过Terraform，写一个配置文件，就可以帮助您购买一台云服务器，或者申请到阿里云Elasticsearch（简称ES）、OSS等云资源。本文介绍通过Terraform管理阿里云ES的方法，包括创建、更新、查看、删除实例等操作。

您可以通过以下两种方式安装并配置Terraform环境：

-   [在本地安装和配置Terraform]()（本文以此为例）。
-   [在Cloud Shell中使用Terraform]()。

## 安装并配置Terraform

1.  前往[Terraform官网](https://www.terraform.io/downloads.html)，下载适用于您的操作系统的程序包。

    本文以Linux系统为例。如果您还没有Linux环境，可购买阿里云ECS实例，详情请参见[步骤一：创建ECS实例](/intl.zh-CN/快速入门/通过控制台创建实例（详细版）/Linux系统实例快速入门.md)。

2.  将程序包解压到/usr/local/bin目录。

    如果您需要将可执行文件解压到其他目录，请按照以下方法为其定义全局路径：

    -   Linux：参见[在Linux系统定义全局路径](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix)
    -   Windows：参见[在Windows系统定义全局路径](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)
    -   Mac：参见[在Mac系统定义全局路径](https://stackoverflow.com/questions/58438609/how-to-set-user-path-permanently-on-mac-os-catalina-zsh-shell)
3.  执行`terraform`命令验证路径配置。

    ```
    terraform
    Usage: terraform [-version] [-help] <command> [args]
    ```

    ![运行terraform](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8212659951/p72175.png)

4.  创建RAM用户，并为其授权。

    为提高权限管理的灵活性和安全性，建议您创建RAM用户，并为其授权。

    1.  登录[RAM控制台](https://ram.console.aliyun.com/#/overview)。

    2.  创建名为Terraform的RAM用户，并为该用户创建AccessKey。

        具体操作方法请参见[创建RAM用户](/intl.zh-CN/用户管理/基本操作/创建RAM用户.md)。

        **说明：** 请不要使用主账号的AccessKey配置Terraform工具。

    3.  为RAM用户授权。

        本示例为用户Terraform授予`AliyunElasticsearchFullAccess`和`AliyunVPCFullAccess`权限，具体操作方法请参见[为RAM用户授权](/intl.zh-CN/用户管理/授权管理/为RAM用户授权.md)。

5.  创建测试目录。

    因为每个Terraform项目都需要创建一个独立的执行目录，所以需要先创建一个测试目录。以下创建一个名为`terraform-test`的测试目录。

    ```
    mkdir terraform-test
    ```

6.  进入`terraform-test`目录。

    ```
    cd terraform-test
    ```

7.  创建配置文件，并配置身份认证信息。

    Terraform在运行时，会读取该目录下所有的\*.tf和\*.tfvars文件。请按照实际需求，将配置信息写入到不同的文件中。以下列出几个常用的配置文件。

    |配置文件|说明|
    |----|--|
    |provider.tf|provider配置。|
    |terraform.tfvars|配置provider要用到的变量。|
    |varable.tf|通用变量。|
    |resource.tf|资源定义。|
    |data.tf|包文件定义。|
    |output.tf|输出文件定义。|

    例如创建provider.tf文件时，可以按照以下格式配置您的身份认证信息。

    ```
    vim provider.tf
    ```

    ```
    provider "alicloud" {
        region      = "cn-hangzhou"
        access_key  = "LTA**********NO2"
        secret_key   = "MOk8x0*********************wwff"
        }
    ```

    更多配置信息请参见[alicloud\_elasticsearch\_instance](https://www.terraform.io/docs/providers/alicloud/r/elasticsearch.html)。

8.  在当前目录下创建plugh目录，下载[provider插件](https://releases.hashicorp.com/terraform-provider-alicloud/1.64.0/)并解压到plugh目录下。

9.  初始化工作目录，使用`-plugin-dir`指定provider所在的路径，完成配置。

    ```
    terraform init -plugin-dir=./plugh/
    ```

    输出`Terraform has been successfully initialized`表示初始化成功。

    **说明：** 每个Terraform项目在新建Terraform工作目录并创建配置文件后，都需要初始化工作目录。


## 创建阿里云ES实例

1.  在测试目录下，创建一个elastic.tf配置文件。

2.  参考以下脚本配置elastic.tf文件，创建一个跨可用区的通用商业版6.7版本的阿里云ES实例。

    ```
    resource "alicloud_elasticsearch_instance" "instance" {
      description          = "testInstanceName"
      instance_charge_type = "PostPaid"
      data_node_amount     = "2"
      data_node_spec       = "elasticsearch.sn2ne.large"
      data_node_disk_size  = "20"
      data_node_disk_type  = "cloud_ssd"
      vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
      password             = "es_password"
      version              = "6.7_with_X-Pack"
      master_node_spec     = "elasticsearch.sn2ne.large"
      zone_count           = "1"
    }
    ```

    provider插件支持的所有参数说明如下。

    |参数|是否必选|描述|
    |--|----|--|
    |`description`|否|实例自定义名称的描述。|
    |`instance_charge_type`|否|计费模式。可选值：`PrePaid`、`PostPaid`（默认）。|
    |`period`|否|购买时长（单位：月），当`instance_charge_type`为`PrePaid`时有效。可选值：1~9、12、24、36，默认是1个月。|
    |`data_node_amount`|是|ES集群的数据节点的个数。可选值：2~50之间。|
    |`data_node_spec`|是|数据节点实例规格。|
    |`data_node_disk_size`|是|指定磁盘空间。不同类型的磁盘，支持的最大存储空间大小不同：     -   `cloud_ssd`：SSD盘，支持最大存储2048GB（2T）。
    -   `cloud_efficiency`：高效云盘，支持最大5T的存储空间，提供较为低廉的存储能力，适合大规模数据量的日志及分析场景。高效云盘超过2048GiB时，只能取：2560、3072、3584、4096、4608、5120。 |
    |`data_node_disk_type`|是|存储类型。可选值：`cloud_ssd`、`cloud_efficiency`。|
    |`vswitch_id`|是|虚拟交换机的实例ID。|
    |`password`|否|实例密码，支持大小写、数字、特殊字符，长度为8~32位字符。特殊字符包括`!@#$%^&*()_+-=`。|
    |`kms_encrypted_password`|否|KMS加密密码。如果配置了`password`，该字段将被忽略。`password`和`kms_encrypted_password`必须配置一个。|
    |`kms_encryption_context`|否|KMS加密上下文。只有设置了`kms_encrypted_password`时才有效。用于对使用`kms_encrypted_password`加密创建或更新的实例进行解密，详情请参见[encryption context](https://www.alibabacloud.com/help/zh/doc-detail/42975.htm)。|
    |`version`|是|ES版本。可选值：`5.5.3_with_X-Pack`、`6.3_with_X-Pack`、`6.7_with_X-Pack`。|
    |`private_whitelist`|否|设置实例的专有网络VPC（Virtual Private Cloud）网络白名单。|
    |`kibana_whitelist`|否|设置Kibana访问白名单。|
    |`master_node_spec`|否|Master节点规格。|
    |`advancedDedicateMaster`|否|用于表示是否创建专有主节点，取值含义如下：    -   true：创建专有主节点。如果部署多可用区并且启用专有主节点，则需要将该参数设置为true。
    -   false（默认值）：不创建专有主节点。 |
    |`zone_count`|否|可用区数量。取值为1~3，`data_node_amount`必须是该值的整数倍。|

    更多参数详情请参见[alicloud\_elasticsearch\_instance](https://www.terraform.io/docs/providers/alicloud/r/elasticsearch.html)。

    **说明：**

    -   `kms_encrypted_password`和`kms_encryption_context`参数要求provider插件版本在1.57.1及以上；`zone_count`参数要求provider插件版本在1.44.0及以上。
    -   如果需要购买除数据节点外的其他属性节点，请参见[createInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/createInstance.md)参数开启其他节点属性。例如：购买多可用区专有主节点，脚本中需要加入`advancedDedicateMaster="true"`。
3.  执行`terraform plan`命令，查看将会执行的操作。

    执行成功后，返回如下结果。

    ```
    # terraform plan
    Refreshing Terraform state in-memory prior to plan...
    The refreshed state will be used to calculate this plan, but will not be
    persisted to local or remote state storage.
    ------------------------------------------------------------------------
    An execution plan has been generated and is shown below.
    Resource actions are indicated with the following symbols:
      + create
    
    Terraform will perform the following actions:
      # alicloud_elasticsearch_instance.instance will be created
      + resource "alicloud_elasticsearch_instance" "instance" {
          + description          = "testInstanceName"
          + data_node_amount     = 2
          + data_node_disk_size  = 20
          + data_node_disk_type  = "cloud_ssd"
          + data_node_spec       = "elasticsearch.sn2ne.large"
          + domain               = (known after apply)
          + id                   = (known after apply)
          + instance_charge_type = "PostPaid"
          + kibana_domain        = (known after apply)
          + kibana_port          = (known after apply)
          + kibana_whitelist     = (known after apply)
          + master_node_spec     = "elasticsearch.sn2ne.large"
          + password             = (sensitive value)
          + port                 = (known after apply)
          + private_whitelist    = (known after apply)
          + public_whitelist     = (known after apply)
          + status               = (known after apply)
          + version              = "6.7_with_X-Pack"
          + vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
          + zone_count           = 1
        }
    
    Plan: 1 to add, 0 to change, 0 to destroy.
    ------------------------------------------------------------------------
    Note: You didn't specify an "-out" parameter to save this plan, so Terraform
    can't guarantee that exactly these actions will be performed if
    "terraform apply" is subsequently run.
    ```

4.  执行`terraform apply`命令，运行工作目录中的配置文件，输入yes。

    执行成功后，返回如下结果。

    ```
    # terraform apply
    Plan: 1 to add, 0 to change, 0 to destroy.
    Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.
      Enter a value: yes
     alicloud_elasticsearch_instance.instance: Creating...
     alicloud_elasticsearch_instance.instance: Still creating... [10s elapsed]
    alicloud_elasticsearch_instance.instance: Still creating... [20s elapsed]
     ...............
     Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
    ```

5.  登录[阿里云ES控制台](https://elasticsearch.console.aliyun.com/)，查看创建成功的ES集群。

    ![创建成功的ES集群](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8212659951/p72229.png)


## 更新资源

1.  进入测试目录，修改elastic.tf配置文件。

    例如修改`data_node_disk_size`为`50`。

    ```
    resource "alicloud_elasticsearch_instance" "instance" {
      instance_charge_type = "PostPaid"
      data_node_amount     = "2"
      data_node_spec       = "elasticsearch.sn2ne.large"
      data_node_disk_size  = "50"
      data_node_disk_type  = "cloud_ssd"
      vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
      password             = "es_password"
      version              = "6.7_with_X-Pack"
      master_node_spec     = "elasticsearch.sn2ne.large"
      zone_count           = "1"
    }
    ```

    **说明：**

    -   创建实例后，`version`无法修改。
    -   每次请求，只支持一项操作修改。例如同时修改`data_node_spec`和`data_node_disk_size`，系统将会出现错误响应。
2.  执行`terraform plan`查看资源信息。

3.  执行`terraform apply`等待资源升配结束。


## 导入阿里云ES资源

如果阿里云ES实例不是通过Terraform创建的，可通过命令，将阿里云ES导入到Terraform的state目录下。

1.  在测试目录下，创建一个main.tf文件。

    ```
    # vim main.tf
    ```

2.  进行资源声明，指定所要导入的资源在state中的存放路径。

    ```
    resource "alicloud_elasticsearch_instance" "test" {}
    ```

3.  开始资源导入操作。

    ```
    # terraform import alicloud_elasticsearch_instance.test  es-cn-0pp1f1y5g000h****
    alicloud_elasticsearch_instance.test: Importing from ID "es-cn-0pp1f1y5g000h****"...
    alicloud_elasticsearch_instance.test: Import prepared!
      Prepared alicloud_elasticsearch_instance for import
    alicloud_elasticsearch_instance.test: Refreshing state... [id=es-cn-0pp1f1y5g000h****]
    
    Import successful!
    
    The resources that were imported are shown above. These resources are now in
    your Terraform state and will henceforth be managed by Terraform.
    ```

    **说明：** 有关import如何实现存量资源的管理，请参见[一文揭秘存量云资源的管理难题](https://yq.aliyun.com/articles/726955)。


## 查看所有被管理的资源

使用`terraform show`命令，查看当前state中所有被管理的资源及其所有属性值。

```
# terraform show
# alicloud_elasticsearch_instance.instance:
resource "alicloud_elasticsearch_instance" "instance" {
    data_node_amount     = 2
    data_node_disk_size  = 20
    data_node_disk_type  = "cloud_ssd"
    data_node_spec       = "elasticsearch.sn2ne.large"
    domain               = "es-cn-dssf9op81lz4q****.elasticsearch.aliyuncs.com"
    id                   = "es-cn-dssf9op81lz4q****"
    instance_charge_type = "PostPaid"
    kibana_domain        = "es-cn-dssf9op81lz4q****.kibana.elasticsearch.aliyuncs.com"
    kibana_port          = 5601
    kibana_whitelist     = []
    master_node_spec     = "elasticsearch.sn2ne.large"
    password             = (sensitive value)
    port                 = 9200
    private_whitelist    = []
    public_whitelist     = []
    status               = "active"
    version              = "6.7.0_with_X-Pack"
    vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
    zone_count           = 1
}

# alicloud_elasticsearch_instance.test:
resource "alicloud_elasticsearch_instance" "test" {
    data_node_amount     = 3
    data_node_disk_size  = 51
    data_node_disk_type  = "cloud_ssd"
    data_node_spec       = "elasticsearch.r5.large"
    domain               = "es-cn-0pp1f1y5g000h****.elasticsearch.aliyuncs.com"
    id                   = "es-cn-0pp1f1y5g000h****"
    instance_charge_type = "PostPaid"
    kibana_domain        = "es-cn-0pp1f1y5g000h****.kibana.elasticsearch.aliyuncs.com"
    kibana_port          = 5601
    kibana_whitelist     = []
    port                 = 9200
    private_whitelist    = []
    public_whitelist     = []
    status               = "active"
    version              = "6.7.0_with_X-Pack"
    vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
    zone_count           = 1

    timeouts {}
}
```

## 删除实例

**警告：** 实例删除后将不可恢复，实例中的所有数据将被清空。

进入测试目录，执行`terraform destroy`命令，输入yes，即可删除该实例。

```
# terraform destroy
alicloud_elasticsearch_instance.instance: Refreshing state... [id=es-cn-v3x49h5397fau****]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # alicloud_elasticsearch_instance.instance will be destroyed
  - resource "alicloud_elasticsearch_instance" "instance" {
      - data_node_amount     = 2 -> null
      - data_node_disk_size  = 20 -> null
      - data_node_disk_type  = "cloud_ssd" -> null
      - data_node_spec       = "elasticsearch.sn2ne.large" -> null
      - domain               = "es-cn-v3x49h5397fau****.elasticsearch.aliyuncs.com" -> null
      - id                   = "es-cn-v3x49h5397fau****" -> null
      - instance_charge_type = "PostPaid" -> null
      - kibana_domain        = "es-cn-v3x49h5397fau****.kibana.elasticsearch.aliyuncs.com" -> null
      - kibana_port          = 5601 -> null
      - kibana_whitelist     = [] -> null
      - master_node_spec     = "elasticsearch.sn2ne.large" -> null
      - password             = (sensitive value)
      - port                 = 9200 -> null
      - private_whitelist    = [] -> null
      - public_whitelist     = [] -> null
      - status               = "active" -> null
      - version              = "6.7.0_with_X-Pack" -> null
      - vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****" -> null
      - zone_count           = 1 -> null
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

alicloud_elasticsearch_instance.instance: Destroying... [id=es-cn-v3x49h5397fau****]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 10s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 20s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 30s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 40s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 50s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 1m0s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 1m10s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 1m20s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 1m30s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 1m40s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 1m50s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 2m0s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 2m10s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 2m20s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 2m30s elapsed]
alicloud_elasticsearch_instance.instance: Still destroying... [id=es-cn-v3x49h5397fau****, 2m40s elapsed]
alicloud_elasticsearch_instance.instance: Destruction complete after 10m2s

Destroy complete! Resources: 1 destroyed.
```

