## Define Infrastructure Templates and Blueprints  

{!docs/common/config-1-overview.md!}  


| Configuration | Description |  Create Cluster |
|---|---|---|
| [Networks](#networks) |(Required) Virtual networks provide the networking infrastructure (network, subnet, Internet gateway, and so on) in which your clusters run. A virtual network on AWS is called Amazon Virtual Private Cloud (Amazon VPC). You can create new virtual networks or reuse existing virtual networks for your clusters. For basic information about VPCs and subnets on AWS, refer to [AWS documentation](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html). | You can select the network configuration for your clusters in the **Create Cluster** wizard > **Set up Network and Security** page. | 
| [Security Groups](#security-groups) | (Required) Security groups include rules which define inbound traffic allowed to the instances in your cluster. You can define different security group configurations for different nodes of your cluster. For basic information about security groups on AWS, refer to [AWS documentation](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html). | You can select the security group configurations for each host group in the **Create Cluster** wizard > **Choose Blueprint** page. If no custom security groups are selected, default is used. | 
| [VMs and Storage](#vms-and-storage) | (Required) "Templates" define the AWS infrastructure for the instances on which your cluster runs. You can select the EC2 instance types and their attached storage, including storage type, size, count, and encryption settings. You can reuse the same template for different cluster host groups or create different templates for different host groups. | You can select a template for each host group in the **Create Cluster** wizard > **Choose Blueprint** page. If no custom templates are selected, default is used. | 
| [Blueprints](#blueprints) | (Required) Blueprints are your declarative definition of a Hadoop cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. | You can select a blueprint for your cluster in the **Create Cluster** wizard > **Choose Blueprint** page. A few default blueprints are available. |   


### Networks

You have four options:

* **Use the default network configuration template**: This requires no further action. Every time a cluster is created with this kind of network setup a new VPC and a new subnet with the specified IP range will be created for the instances on AWS.  
* **Create a new VPC and a new subnet**: Every time a cluster is created with this kind of network setup a new VPC and a new subnet with the specified IP range will be created for the instances on AWS.  
* **Create a new subnet in an existing VPC**: Use this option if you already have a VPC on AWS where you'd like to put the cluster but you'd like to have a separate subnet for it. This setup is only supported for basic VPCs, where an Internet Gateway is configured and instances have public IP addresses to access the Internet.        
* **Use an existing subnet in an existing VPC**: Use this option (1) If you have an existing VPC with one or more subnets on AWS and you'd like to start the cluster in one (or more) of these subnets. (2) If you have a custom VPC setup (VGW, NAT, private subnets, and so on).  In this setup, instances in the subnet don't need to have public IP addresses. If using multiple subnets, the subnets can be in different availability zones.    

If you are planning to use an existing VPC, consider these restrictions:

* The subnet CIDRs cannot overlap in a VPC. Make sure that the new subnets that you define don't overlap with already deployed subnets in that VPC.  
* Since subnet CIDRs cannot overlap in a VPC, if you want to launch multiple clusters in the same VPC, you must create a different network template for each cluster. For example, you can create three different clusters with three different network templates for multiple subnets (`10.0.0.0/24`, `10.0.1.0/24`, `10.0.2.0/24` respectively) with the same VPC and IGW identifiers.  

If using an existing subnet, consider these restrictions: 

* Before using an existing subnet, make sure you have enough room within your network space for the new instances.   
* Instances in the subnet must be able to reach the Internet to download yum packages (this can be done through a Virtual Gateway, a NAT instance, an Internet Gateway, or by other means).  
* The VM on which Cloudbreak is deployed must be able to reach the instances in the cluster on port 443, either by being in the same subnet or through a router from another subnet.  


#### Default Network

Cloudbreak includes one pre-defined network configuration called *default-aws-network*, which is used by default when creating a cluster.  You can see it in the **manage networks** tab. The configuration is: 

| Parameter | Value |
|---|---|
| Name | default-aws-network |
| Description | Default network settings for AWS clusters. |
| Subnet (CIDR) | 10.0.0.0/16 |

With this default configuration, a new VPC with a 10.0.0.0/16 subnet will be created every time a cluster is created. No resources will be created until you create a cluster using this configuration.


#### Add New VPC and Subnet 

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Create a new VPC and a new subnet**.

2. Provide required parameters:

    | Parameter | Description |
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Subnet (CIDR) | Enter a valid [CIDR](http://www.ipaddressguide.com/cidr) for a new subnet that will be created within the VPC. For example `10.0.0.0/24`. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.


#### Add New Subnet in Existing VPC

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Create a new subnet in an existing VPC**.

2. Provide required parameters: 

    | Parameter | Description |
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| VPC Identifier | Paste the "VPC ID" of your existing VPC as it appears in your VPC Dashboard on AWS. The "VPC ID" should be in the format "vpc-12345678".  |
| Internet Gateway Identifier | Paste the "ID" of the Internet Gateway associated with the chosen VPC as it appears in your VPC Dashboard on AWS. The "ID" should be in the format "igw-12345678". |
| Subnet (CIDR) | Enter a [CIDR](http://www.ipaddressguide.com/cidr) for the new subnet that will be created within the VPC. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.


#### Use Existing VPC and Subnet  

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Use an existing subnet in an existing VPC**. 

2. Provide required parameters: 

    | Parameter | Description |
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description.  |
| VPC Identifier | Paste the "VPC ID" of your existing VPC as it appears in your VPC Dashboard on AWS. The "VPC ID" should be in the format "vpc-12345678". |
| Subnet Identifier | Paste the "Subnet ID" of your existing subnet as it appears in your VPC Dashboard on AWS. The subnet must be within the VPC specified above. The 'Subnet ID" should be in the format "subnet-12345678". You can set a single subnet or a comma separated list of subnets. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.
    
    

### Security Groups

You have three options:

* **Use the default security group configuration**: This requires no further action.  
* **Add your own custom security group configuration**: You can define your own security group by adding the ports, protocols, and [CIDR](http://www.ipaddressguide.com/cidr) ranges that you'd like to use. The rules defined here don't need to contain the internal rules (these are automatically added by Cloudbreak to the security group on AWS).  
* **Copy rules from an existing security group**: Use this option if you have an existing security group and you'd like to apply the same rules. 

You can define different security group configurations for different nodes of your cluster.


#### Default Security Group

Cloudbreak includes one pre-defined security group configuration called *default-aws-only-ssh-and-ssl*, which is used by default when creating a cluster. You can see it in the **manage security groups** tab. The configuration is: 

{!docs/common/config-sg.md!} 


#### Use Existing Security Group  

You can copy rules of an existing security group by using the **Use an existing security group** option available in the **manage security groups** tab: 

1. Click on **Use an existing security group**.

2. Provide required parameters:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your configuration.|
| Description | (Optional) Enter a description.|
| Security Group Identifier | Enter the "Group ID" of an existing security group whose settings you would like to reuse. The "Group ID" should be in the following format "sg-12345678". | 
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this security group to create clusters, but they cannot delete it. |  

    > Ports 22, 443, and 9443 must be open on every security group; otherwise Cloudbreak will not be able to communicate with your provisioned cluster.

3. Click **+create security group** to save the configuration. 

    No resources will be created until you create a cluster using this configuration. 
    
> Ports used by Hadoop services: Ambari (8080) Consul (8500) NN (50070) RM Web (8088) Scheduler (8030RM) IPC (8050RM) Job history server (19888) HBase master (60000) HBase master web (60010) HBase RS (16020) HBase RS info (60030) Falcon (15000) Storm (8744) Hive metastore (9083) Hive server (10000) Hive server HTTP (10001) Accumulo master (9999) Accumulo Tserver (9997) Atlas (21000) KNOX (8443) Oozie (11000) Spark HS (18080) NM Web (8042) Zeppelin WebSocket (9996) Zeppelin UI (9995) Kibana (3080) Elasticsearch (9200)
    
    

### VMs and Storage 

You have two options:

* **Use the default template**: This requires no further action.  
* **Add your own custom template**: Use this option if you have specific requirements for your cluster node infrastructure. A typical setup may combine multiple templates in a cluster for the different types of nodes. For example you may want to attach multiple large disks to the data nodes or have memory optimized instances for Spark nodes.   

#### Default Template

Cloudbreak includes one pre-defined template called *minviable-aws*, which is used by default when creating a cluster. You can see it in the **manage templates** tab. The configuration is:

| Parameter | Value |
|---|---|
| Name | minviable-aws |
| Instance Type | m3.large |
| Volume Type | General Purpose (SSD) |
| Attached Volumes Per Instance | 1 |
| Volume Size (GB) | 100 |
| EBS Encryption | False |


#### Add Custom Template 

You can define reusable configurations in the **manage templates** tab: 

1. Click on **+create template**.

2. Provide required parameters: 

    | Parameter | Description |
|---|---|
| Name | Enter a name for your template. |
| Description | (Optional) Enter a description. |
| Instance Type | Select an instance type. For more information about instance types on AWS refer to [AWS documentation](https://aws.amazon.com/ec2/instance-types/). |
| <p>Volume Type</p> | <p>Select the volume type. The options are:<ul><li>Magnetic</li><li>Ephemeral</li><li>General Purpose (SSD)</li><li>Throughput Optimized HDD</li></ul>For more information about these options refer to <a href="http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html" target="_blank">AWS documentation</a>.</p>|
| Attached Volumes Per Instance | Enter the number of volumes attached per instance. Default is 1. |
| Volume Size (GB) | Enter the size in GBs for each volume. Default is 100. |
| Spot Price (USD) | If you would like to use spot instances, enter your bid. Cloudbreak will request spot price instances (which might take a while or never be fulfilled by Amazon). This option is not supported by the default RedHat images. |
| EBS Encryption | Check the box to enable EBS encryption. If this option is checked, then all the attached disks will be encrypted using the AWS KMS master keys. Refer to [AWS documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html). |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create template**.

    No resources will be created until you create a cluster using this configuration.
    


### Blueprints 

{!docs/common/blueprints.md!}  




<div class="next">
<a href="../aws-create/index.html">Next: Create a Cluster</a>
</div>