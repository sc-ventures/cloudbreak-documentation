## Creating a cluster on Azure 

{!docs/common/create-a1.md!}
| Region | Select the Azure region in which you would like to launch your cluster. For information on available Azure regions, refer to [Azure documentation](https://azure.microsoft.com/en-us/regions/). |
{!docs/common/create-a2.md!}
| Instance Type | Select an instance type. For information about instance types on Azure refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general). |
{!docs/common/create-a3-0.md!}
| Storage Type | <p>Select the volume type. The options are:<ul><li>Locally-redundant storage</li><li>Geo-redundant storage</li><li>Premium locally-redundant storage</li></ul> For more information about these options refer to <a href="https://docs.microsoft.com/en-us/azure/storage/storage-introduction" target="_blank">Azure documentation</a>. |
{!docs/common/create-a3-1a.md!}
{!docs/common/create-a3-1b.md!}
{!docs/common/create-a3-2.md!}
{!docs/common/create-a4.md!}
{!docs/common/create-a5.md!}

**Related links**  
[Flex support subscription](get-help.md#flex-subscription)  
[Using custom blueprints](blueprints.md)   
[Default cluster security groups](security.md#default-cluster-security-groups)  
[Troubleshooting cluster creation](trouble-cluster.md)   
[Azure regions](https://azure.microsoft.com/en-us/regions/) (External)     
[CIDR](http://www.ipaddressguide.com/cidr) (External)  
[General purpose Linux VM sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general) (External)  
 


### Advanced cluster options

{!docs/common/create-adv-1.md!}

 
{!docs/common/create-adv-2.md!}


#### Root volume size 

Use this option to increase the root volume size. Default is 30 GB. This option is useful if your custom image requires more space than the default 30 GB.

If you change the value of the root volume size, an osDisk with the given rootVolumeSize will be created for the instance automatically; However, you will have to manually resize the osDisk partition by using the steps provided in the [Azure documentation](https://blogs.msdn.microsoft.com/linuxonazure/2017/04/03/how-to-resize-linux-osdisk-partition-on-azure/).

**Related links**  
[How to: Resize Linux osDisk partition on Azure](https://blogs.msdn.microsoft.com/linuxonazure/2017/04/03/how-to-resize-linux-osdisk-partition-on-azure/)  


#### Availability sets 

To support fault tolerance for VMs, Azure uses the concept of [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability). This allows two or more VMs to be mapped to multiple fault domains, each of which defines a group of virtual machines that share a common power source and a network switch. When adding VMs to an availability set, Azure automatically assigns each VM a fault domain. The SLA includes guarantees that during OS Patching in Azure or during maintenance operations, at least one VM belonging to a given fault domain will be available.

In Cloudbreak, an availability set is automatically configured during cluster creation for each host group with "Instance Count" that is set to 1 or larger. The assignment of fault domains is automated by Azure, so there is no option for this in Cloudbreak UI. 
    
Cloudbreak allows you to configure the availability set on the advanced **Hardware and Storage** page of the create cluster wizard by providing the following options for each host group:  

| Parameter | Description | Default |
|---|---|---|
| Availability Set Name | Choose a name for the availability set that will be created for the selected host group | The following convention is used: "clustername-hostgroupname-as" |
| Fault Domain Count | The number of fault domains. | 2 or 3, depending on the setting supported by Azure  |
| Update Domain Count | This number of update domains. This can be set to a number in range of 2-20. | 20 |

After the deployment is finished, you can check the layout of the VMs inside an availability set on Azure Portal. You will find the "Availability set" resources corresponding to the host groups inside the deployment's resource group.


#### Cloud storage 

If you would like to access ADLS or WASB from your cluster, you must configure access as described in [Configuring access to ADLS](azure-data-adls.md) or [Configuring access to WASB](azure-data-wasb.md). 

**Related links**  
[Configuring access to ADLS](azure-data-adls.md)  
[Configuring access to WASB](azure-data-wasb.md)  


{!docs/common/create-adv-4.md!} 


#### Don't create public IP

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to use public IPs for the network. 


#### Don't create new firewall rules

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to create new firewall rules for the network. 

{!docs/common/create-adv-5.md!}


#### Enable Azure disk encryption 

Check this option if you would like to have your virtual machine disks encrypted using the Azure Disk Encryption capability provided by Azure. For more information, refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption).  

{!docs/common/create-adv-6.md!}  
[Introduction to Microsoft Azure storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction) (External)  



<div class="next">
<a href="../azure-data-adls/index.html">Next: Configure Access to ADLS</a>
</div>

