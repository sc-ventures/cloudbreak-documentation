## Creating a Cluster on Azure 

{!docs/common/create-a1.md!}
| Region | Select the Azure region in which you would like to launch your cluster. For information on available Azure regions, refer to [Azure documentation](https://azure.microsoft.com/en-us/regions/). |
{!docs/common/create-a2.md!}
| Instance Type | Select an instance type. For information about instance types on Azure refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general). |
{!docs/common/create-a3.md!}
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

[Comment]: <> (Will update this once I can see it the UI how it works)


To support fault tolerance for VMs, Azure uses the concept of [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability). This allows two or more VMs to be mapped to multiple fault domains, each of which defines a group of virtual machines that share a common power source and a network switch. When adding VMs to an availability set, Azure automatically assigns each VM a fault domain. The SLA includes guarantees that during OS Patching in Azure or during maintenance operations, at least one VM belonging to a given fault domain will be available.

In Cloudbreak UI, availability sets can be configured during cluster creation for each host group:

1. For a given host group, enable availability sets by checking the box.  

    > The availability set option is only available for non-Ambari host groups and is only available when "Instance Count" is set to 2 or a larger number. One availability set can be assigned to only one host group. The assignment of fault domains is automated by Azure, so there is no option for this in Cloudbreak UI. 
    
2. Add the desired availability set by providing the following information: 

    | Parameter | Description |
|---|---|
| Availability Set Name | Choose a name for the availability set that will be created for the selected host group. |
| Fault Domain Count | By, default, the fault domain count is set to 2. To update this number, provide a desired domain count by updating the "Update Domain Count" parameter.|
| Update Domain Count | To update the fault domain count, provide a desired domain count by updating the "Update Domain Count" parameter |

After the deployment is finished, you can check the layout of the VMs inside an availability set on Azure Portal. You will find the "Availability set" resources corresponding to the host groups inside the deployment's resource group.


{!docs/common/create-adv-4.md!} 


#### Don't Create Public IP

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to use public IPs for the network. 


#### Don't Create New Firewall Rules

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to create new firewall rules for the network. 


#### Enable Azure Disk Encryption 

Check this option if you would like to have your virtual machine disks encrypted using the Azure Disk Encryption capability provided by Azure. For more information, refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption).  

{!docs/common/create-adv-5.md!}  
[Introduction to Microsoft Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction) (External)  



<div class="next">
<a href="../azure-clusters-access/index.html">Next: Access Cluster</a>
</div>

