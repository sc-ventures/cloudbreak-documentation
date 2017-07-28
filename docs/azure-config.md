## Define Infrastructure Templates and Blueprints 

{!docs/common/config-1-overview.md!}    


| Configuration | Description |  Create Cluster |
|---|---|---|
| [Networks](#networks) |(Required) Virtual networks provide the networking infrastructure (network, subnet, Internet gateway, and so on) in which your clusters run. You can create new virtual networks or reuse existing virtual networks for your clusters. Virtual networks on Azure are called Azure Virtual Networks (VNets). For basic information about VNets, refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview). | You can select the network configuration for your clusters in the **Create Cluster** wizard > **Set up Network and Security** page. If no custom network is selected, default is used. | 
| [Security Groups](#security-groups) | (Required) Security groups include rules which define inbound traffic allowed to the instances in your cluster. You can define different security group configurations for different nodes of your cluster. For basic information about security groups on Azure, refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg). | You can select the security group configurations for each host group in the **Create Cluster** wizard > **Choose Blueprint** page. If no custom security groups are selected, default is used. | 
| [VMs and Storage](#vms-and-storage) | (Required) "Templates" define the Azure infrastructure for the instances on which your cluster runs. You can select the VM types and their attached storage, including storage type, size, and count. You can reuse the same template for different cluster host groups or create different templates for different host groups. | You can select infrastructure templates for each host group in the **Create Cluster** wizard > **Choose Blueprint** page. If no custom templates are selected, default is used. | 
| [Blueprints](#blueprints) | (Required) Blueprints are your declarative definition of a Hadoop cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. | You can select a blueprint for your cluster in the **Create Cluster** wizard > **Choose Blueprint** page. A few default blueprints are available. |  



### Networks

You have three options:

* **Use the default network configuration template**: This requires no further action. Every time a cluster is created with this kind of network setup a new virtual network and a new subnet with the specified IP range will be created for the instances on Azure.  
* **Create a new virtual network and a new subnet**: Every time a cluster is created with this kind of network setup a new virtual network and a new subnet with the specified IP range will be created for the instances on Azure.          
* **Use an existing subnet in an existing virtual network**: Use this option if you have an existing virtual network with one or more subnets on Azure and you'd like to start the instances of a cluster in one of those subnets.

> If you are using an existing subnet, ensure that you have opened all the additional ports required for accessing your cluster in the security group of your subnet.

#### Default Network

Cloudbreak includes one pre-defined network configuration called *default-azure-network*, which is used by default when creating a cluster.  You can see it in the **manage networks** tab. The configuration is: 

|Parameter | Value |  
|---|---|
| Name | default-azure-network|
| Description | Default network settings for Azure clusters.|
| Subnet (CIDR) | 10.0.0.0/16|

With this default configuration, a new virtual network with a 10.0.0.0/16 subnet will be created every time a cluster is created. No resources will be created until you create a cluster using this configuration.


#### Add New Network and Subnet

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Create a new virtual network and a new subnet**.

2. Provide required parameters:

    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Subnet (CIDR) | Enter a valid [CIDR](http://www.ipaddressguide.com/cidr) for a new subnet that will be created within the VNet. For example `10.0.0.0/24`. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.

#### Use Existing Network and Subnet

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Use an existing subnet in an existing virtual network**. 

2. Provide required parameters: 

    > Make sure that the new subnet defined here doesn't overlap with any of your already deployed subnets in the network. 

    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Resource Group Identifier | Paste the name of the resource group in which the network is located on Azure. You can obtain it from your Azure portal by navigating to the "Properties" of your virtual network and coping the value of the "NAME" property. | 
| Virtual Network Identifier | Paste the name of your existing VNet. You can obtain it from your Azure Portal by coping the value of the "NAME" property. |
| Subnet Identifier | Paste the name of your existing subnet. You can obtain it from your Azure Portal  by coping the value of the "NAME" property.  |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Don't create public IPs | If you enable this option, Cloudbreak will not assign public IP address to the VMs. You must make sure that Cloudbreak can access the launched instances and that the instances can reach the internet. |
| Don't create new firewall rules | If you enable this option, Cloudbreak will not create security groups. You must make sure (by openining every port in the subnet) that the created instances in the subnet can reach each other. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.
    
    
### Security Groups

You have two options:

* **Use the default security group configuration**: This requires no further action.  
* **Add your own custom security group configuration**: You can define your own security group by adding the ports, protocols, and CIDR ranges that you'd like to use. The rules defined here don't need to contain the internal rules (these are automatically added by Cloudbreak to the security group on Azure). 

You can define different security group configurations for different nodes of your cluster.

#### Default Security Group

Cloudbreak includes one pre-defined security group configuration called *default-azure-only-ssh-and-ssl*, which is used by default when creating a cluster. You can see it in the **manage security groups** tab. The configuration is: 

{!docs/common/config-sg.md!} 
    
> Ports used by Hadoop services: Ambari (8080) Consul (8500) NN (50070) RM Web (8088) Scheduler (8030RM) IPC (8050RM) Job history server (19888) HBase master (60000) HBase master web (60010) HBase RS (16020) HBase RS info (60030) Falcon (15000) Storm (8744) Hive metastore (9083) Hive server (10000) Hive server HTTP (10001) Accumulo master (9999) Accumulo Tserver (9997) Atlas (21000) KNOX (8443) Oozie (11000) Spark HS (18080) NM Web (8042) Zeppelin WebSocket (9996) Zeppelin UI (9995) Kibana (3080)  Elasticsearch (9200)


### VMs and Storage 

You have two options:

* **Use the default infrastructure template**: This requires no further action.  
* **Add your own custom infrastructure template**: Use this option if you have specific infrastructure requirements. A typical setup may combine multiple templates in a cluster for the different types of nodes. For example you may want to attach multiple large disks to the data nodes or have memory optimized instances for Spark nodes. 

#### Default Template

Cloudbreak includes one pre-defined infrastructure template called *minviable-azure*, which is used by default when creating a cluster. You can see it in the **manage templates** tab. The configuration is:


|Parameter | Value |  
|---|---|
| Name | minviable-azure |
| VM Type | Standard_D4 |
| Volume Type | Locally-redundant storage |
| Attached Volumes Per Instance | 1 |
| Volume Size (GB) | 100 |

#### Add Custom Template 

You can define reusable cluster templates in the **manage templates** tab: 

1. Click on **+create template**.

2. Provide required parameters: 

    | Parameter | Description |
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Instance Type | Select a VM type. For more information about instance types on Azure refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general). |
| Volume Type | <p>Select the volume type. The options are:<ul><li>Locally-redundant storage</li><li>Geo-redundant storage</li><li>Premium locally-redundant storage</li></ul> For more information about these options refer to <a href="https://docs.microsoft.com/en-us/azure/storage/storage-introduction" target="_blank">Azure documentation</a>. |
| Attached Volumes Per Instance | Enter the number of volumes attached per instance. Default is 1. |
| Managed disk | Select whether to use managed or unmanaged disk. Only "Locally-redundant storage" volume type is supported for managed disks. Disk sizes will be rounded as described in [Azure documentation](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#pricing-and-billing). For more information on managed disks refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview). |
| Volume Size (GB) | Enter the size in GBs for each volume. Default is 100. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create template**.

    No resources will be created until you create a cluster using this configuration.


### Blueprints 

{!docs/common/blueprints.md!}  




<div class="next">
<a href="../azure-create/index.html">Next: Create a Cluster</a>
</div>


