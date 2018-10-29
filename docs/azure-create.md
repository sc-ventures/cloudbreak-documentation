## Creating a Cluster on Azure 

{!docs/common/create-a1.md!}
| Region | Select the Azure region in which you would like to launch your cluster. For information on available Azure regions, refer to [Azure documentation](https://azure.microsoft.com/en-us/regions/). |
{!docs/common/create-a2.md!}
| Instance Type | Select an instance type. For information about instance types on Azure refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general). |
{!docs/common/create-a3-1.md!}
{!docs/common/create-a3-2.md!}
{!docs/common/create-a4.md!}
{!docs/common/create-a5.md!}

**Related Links**  
[Blueprints](blueprints.md)   
[Default Cluster Security Groups](security.md#default-cluster-security-groups)   
[Azure Regions](https://azure.microsoft.com/en-us/regions/) (External)     
[CIDR](http://www.ipaddressguide.com/cidr) (External)  
[General Purpose Linux VM Sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general) (External)  



### Advanced Options

{!docs/common/create-adv-1.md!}

 
{!docs/common/create-adv-2.md!}
| Storage Type | <p>Select the volume type. The options are:<ul><li>Locally-redundant storage</li><li>Geo-redundant storage</li><li>Premium locally-redundant storage</li></ul> For more information about these options refer to <a href="https://docs.microsoft.com/en-us/azure/storage/storage-introduction" target="_blank">Azure documentation</a>. |
{!docs/common/create-adv-3.md!}

#### Availability Sets 

To support fault tolerance for VMs, Azure uses the concept of [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability). This allows two or more VMs to be mapped to multiple fault domains, each of which defines a group of virtual machines that share a common power source and a network switch. When adding VMs to an availability set, Azure automatically assigns each VM a fault domain. The SLA includes guarantees that during OS Patching in Azure or during maintenance operations, at least one VM belonging to a given fault domain will be available.

In Cloudbreak, an availability set is automatically configured during cluster creation for each non-Ambari host group with "Instance Count" that is set to 2 or larger. The assignment of fault domains is automated by Azure, so there is no option for this in Cloudbreak UI. 
    
Cloudbreak allows you to configure the availability set on the advanced **Hardware and Storage** page of the create cluster wizard by providing the following options for each host group:  

| Parameter | Description | Default |
|---|---|---|
| Availability Set Name | Choose a name for the availability set that will be created for the selected host group | The following convention is used: "clustername-hostgroupname-as" |
| Fault Domain Count | The number of fault domains. | 2 or 3, depending on the setting supported by Azure  |
| Update Domain Count | This number of update domains. This can be set to a number in range of 2-20. | 20 |

After the deployment is finished, you can check the layout of the VMs inside an availability set on Azure Portal. You will find the "Availability set" resources corresponding to the host groups inside the deployment's resource group.


{!docs/common/create-adv-4.md!} 


#### Don't Create Public IP

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to use public IPs for the network. 


#### Don't Create New Firewall Rules

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to create new firewall rules for the network. 

{!docs/common/create-adv-5.md!}


{!docs/common/create-adv-6.md!}  
[Introduction to Microsoft Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction) (External)  



<div class="next">
<a href="../azure-clusters-access/index.html">Next: Access Cluster</a>
</div>

