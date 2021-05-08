# Best practice of Alibaba Cloud Elasticsearch management based on Terraform

This topic describes how to use Terraform to manage your Alibaba Cloud Elasticsearch clusters, such as how to create, update, view, and delete a cluster.

Terraform allows you to use code to allocate resources such as physical machines. You can use Terraform to write a configuration file to purchase a cloud server or apply for resources such as Alibaba Cloud Elasticsearch and Object Storage Service \(OSS\).

You can install and configure Terraform by using either of the following methods:

-   [Install and configure Terraform in the local PC]().

    This method is used in this topic.

-   [Use Terraform in Cloud Shell]().

## Install and configure Terraform

1.  Download the software package applicable to your operating system from [Download Terraform](https://www.terraform.io/downloads.html).

    In this topic, Terraform is installed and configured in a Linux operating system. If you do not have a Linux operating system, purchase an Alibaba Cloud Elastic Compute Service \(ECS\) instance. For more information, see [Step 1: Create an ECS instance](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Linux instances.md).

2.  Decompress the package to /usr/local/bin.

    If you want to decompress the package \(an executable file\) to another directory, define a global path for the file as follows:

    -   Linux: See [How to define a global path on Linux](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix).
    -   Windows: See [How to define a global path on Windows](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).
    -   Mac: See [How to define a global path on Mac](https://stackoverflow.com/questions/58438609/how-to-set-user-path-permanently-on-mac-os-catalina-zsh-shell).
3.  Run the `terraform` command to verify the path.

    ```
    terraform
    Usage: terraform [-version] [-help] <command> [args]
    ```

    ![Run the terraform command](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2757359951/p72175.png)

4.  Create a RAM user and grant permissions to the user.

    For higher flexibility and security in permission management, we recommend that you create a RAM user and grant permissions to the user.

    1.  Log on to the [RAM console](https://ram.console.aliyun.com/#/overview).

    2.  Create a RAM user named Terraform and an AccessKey pair for the user.

        For more information, see [Create a RAM user](/intl.en-US/RAM User Management/Create a RAM user.md).

        **Note:** For security reasons, we recommend that you do not use the AccessKey pair of your Alibaba Cloud account to configure Terraform.

    3.  Authorize the RAM user.

        In this example, the user Terraform is granted `AliyunElasticsearchFullAccess` and `AliyunVPCFullAccess` permissions. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

5.  Create a test directory.

    You must create a separate directory for each Terraform project. In this example, create a test directory named `terraform-test`.

    ```
    mkdir terraform-test
    ```

6.  Go to the `terraform-test` directory.

    ```
    cd terraform-test
    ```

7.  Create a configuration file and configure identity authentication information.

    Terraform reads all \*.tf and \*.tfvars files in the directory when it is running. You can write different configurations to these files as required. The following table lists the configuration files that are frequently used.

    |Configuration file|Description|
    |------------------|-----------|
    |provider.tf|Used to configure providers.|
    |terraform.tfvars|Used to configure the variables required to configure providers.|
    |varable.tf|Used to configure universal variables.|
    |resource.tf|Used to define resources.|
    |data.tf|Used to define package files.|
    |output.tf|Used to define output files.|

    For example, when you create the provider.tf file, configure your identity authentication information in the following format:

    ```
    vim provider.tf
    ```

    ```
    provider "alicloud" {
        region = "cn-hangzhou"
        access_key  = "LTA**********NO2"
        secret_key   = "MOk8x0*********************wwff"
        }
    ```

    For more information, see [alicloud\_elasticsearch\_instance](https://www.terraform.io/docs/providers/alicloud/r/elasticsearch.html).

8.  Create the plugh directory in the current directory, download a provider package, and decompress the package to the plugh directory.

    For more information, see [providers](https://releases.hashicorp.com/terraform-provider-alicloud/1.64.0/).

9.  Initialize the working directory \(the plugh directory\) and use `-plugin-dir` to specify the path for storing the provider.

    ```
    terraform init -plugin-dir=./plugh/
    ```

    If `Terraform has been successfully initialized` is returned, Terraform is initialized.

    **Note:** After you create a working directory and the configuration file for a Terraform project, you must initialize the working directory.


## Create an Elasticsearch cluster

1.  Create an elastic.tf configuration file in the test directory.

2.  Configure the elastic.tf file to create a cross-zone cluster of Elasticsearch V6.7.

    ```
    resource "alicloud_elasticsearch_instance" "instance" {
      instance_charge_type = "PostPaid"
      data_node_amount     = "2"
      data_node_spec       = "elasticsearch.sn2ne.large"
      data_node_disk_size  = "20"
      data_node_disk_type  = "cloud_ssd"
      vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
      password             = "es_password"
      version              = "6.7_with_X-Pack"
      master_node_spec     = "elasticsearch.sn2ne.large"
      zone_count           = "2"
    }
    ```

    The following table describes the parameters supported by providers.

    |Parameter|Required?|Description|
    |---------|---------|-----------|
    |`instance_charge_type`|No|The billing method. Valid values: `PrePaid` and `PostPaid` \(default value\).|
    |`period`|No|The billing cycle for the cluster. Unit: months. This parameter is only valid when `instance_charge_type` is set to `PrePaid`. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36. Default value: 1.|
    |`data_node_amount`|Yes|The number of data nodes in the cluster. Valid values: 2 to 50.|
    |`data_node_spec`|Yes|The specification of a data node.|
    |`data_node_disk_size`|Yes|The disk space. Different types of disks provide different storage space:     -   `cloud_ssd`: When the disk type is standard SSD \(cloud\_ssd\), the maximum value of this parameter is 2048, which indicates 2 TB of storage space.
    -   `cloud_efficiency`: When the disk type is ultra disk \(cloud\_efficiency\), the maximum value of this parameter is 5120, which indicates 5 TB of storage space. Ultra disks are cost-effective and can be used in scenarios such as logging and analyzing large amounts of data. If you want to specify a size greater than 2,048 GB for an ultra disk, you can set the value of this parameter only to 2560, 3072, 3584, 4096, 4608, or 5120. |
    |`data_node_disk_type`|Yes|The type of the disk. Valid values: `cloud_ssd` and `cloud_efficiency`.|
    |`vswitch_id`|Yes|The ID of the VSwitch.|
    |`password`|No|The cluster password. It can be 8 to 32 characters in length and can contain letters, digits, and special characters. Special characters include `! @ # $ % ^ & * ( ) _ + - =`|
    |`kms_encrypted_password`|No|The encrypted password of Key Management Service \(KMS\). If you set `password`, this field is ignored. You must set either `password` or `kms_encrypted_password`.|
    |`kms_encryption_context`|No|The KMS encryption context. This parameter is only valid when `kms_encrypted_password` is set. This parameter is used to decrypt the cluster that is created or updated by using `kms_encrypted_password`. For more information, see [Encryption context](https://www.alibabacloud.com/help/zh/doc-detail/42975.htm).|
    |`version`|Yes|The version of Elasticsearch. Valid values: `5.5.3_with_X-Pack`, `6.3_with_X-Pack`, and `6.7_with_X-Pack`.|
    |`private_whitelist`|No|The cluster whitelist that allows access from a Virtual Private Cloud \(VPC\).|
    |`kibana_whitelist`|No|The Kibana whitelist.|
    |`master_node_spec`|No|The specification of dedicated master nodes.|
    |`zone_count`|No|The number of zones. Valid values: 1 to 3. The value of `data_node_amount` must be an integral multiple of this value.|

    For more information, see [alicloud\_elasticsearch\_instance](https://www.terraform.io/docs/providers/alicloud/r/elasticsearch.html).

    **Note:** `kms_encrypted_password` and `kms_encryption_context` are only available for provider 1.57.1 or later. `zone_count` is only available for provider 1.44.0 or later.

3.  Run the `terraform plan` command to view the operations to be performed.

    If the command is successfully executed, the following result is returned:

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
          + data_node_amount     = 2
          + data_node_disk_size  = 20
          + data_node_disk_type  = "cloud_ssd"
          + data_node_spec       = "elasticsearch.sn2ne.large"
          + domain               = (known after apply)
          + id                = (known after apply)
          + worker_instance_charge_type = "PostPaid"
          + id          = (known after apply)
          + vpc_id            = (known after apply)
          + kibana_whitelist     = (known after apply)
          + master_node_spec     = "elasticsearch.sn2ne.large"
          + password             = (sensitive value)
          + port                 = (known after apply)
          + private_whitelist    = (known after apply)
          + public_whitelist     = (known after apply)
          + status               = (known after apply)
          + version              = "6.7_with_X-Pack"
          + vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
          + zone_count           = 2
        }
    
    Plan: 1 to add, 0 to change, 0 to destroy.
    ------------------------------------------------------------------------
    Note: You didn't specify an "-out" parameter to save this plan, so Terraform
    can't guarantee that exactly these actions will be performed if
    "terraform apply" is subsequently run.
    ```

4.  Run the `terraform apply` command to execute the configuration file in the working directory and enter yes.

    If the command is successfully executed, the following result is returned:

    ```
    # terraform apply
    Plan: 1 to add, 0 to change, 0 to destroy.
    Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.
      Enter a value: yes
     alicloud_db_instance.instance: Creating...
     alicloud_db_instance.instance: Still creating... [10s elapsed]
    alicloud_db_instance.instance: Still creating... [20s elapsed]
     ...............
     Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
    ```

5.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/) to view the created Elasticsearch cluster.

    ![The created Elasticsearch cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9340216061/p72229.png)


## Change resource configurations

1.  Go to the test directory and modify the elastic.tf configuration file.

    For example, change `data_node_disk_size` to `50`.

    ```
    resource "alicloud_db_instance" "instance" {
      instance_charge_type: "" => "PostPaid"
      data_node_amount     = "2"
      data_node_spec       = "elasticsearch.sn2ne.large"
      data_node_disk_size  = "50"
      data_node_disk_type  = "cloud_ssd"
      vswitch_id           = "vsw-bp1f7r0ma00pf9h2l****"
      password             = "es_password"
      version              = "6.7_with_X-Pack"
      master_node_spec     = "elasticsearch.sn2ne.large"
      zone_count           = "2"
    }
    ```

    **Note:**

    -   After the cluster is created, you cannot change the value of `version`.
    -   You can modify only one configuration item in a request. For example, if you modify `data_node_spec` and `data_node_disk_size` at the same time, an error is reported.
2.  Run the `terraform plan` command to view resource information.

3.  Run the `terraform apply` command. Wait until the configuration upgrade of resources is complete.


## Import Elasticsearch resources

If your Elasticsearch cluster is not created by using Terraform, you can run commands to import the cluster to the state directory of Terraform.

1.  Create a file named main.tf in the test directory.

    ```
    # vim main.tf
    ```

2.  Declare resources and specify the storage position of the resources in the state directory.

    ```
    resource "alicloud_elasticsearch_instance" "test" {}
    ```

3.  Import resources.

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

    **Note:** For more information about how to import and manage existing resources, see [Management of existing cloud resources](https://yq.aliyun.com/articles/726955).


## View all managed resources

Run the `terraform show` command to view all the managed resources and their property values in the current state directory.

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
    zone_count           = 2
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

## Delete the cluster

Go to the test directory, run the `terraform destroy` command, and enter yes to delete the cluster.

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
      - zone_count           = 2 -> null
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

