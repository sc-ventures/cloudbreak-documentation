## Creating a Cluster on Azure 

{!docs/common/create-a1.md!}
| Region | Select the Azure region in which you would like to launch your cluster. For information on available Azure regions, refer to [Azure documentation](https://azure.microsoft.com/en-us/regions/). |
{!docs/common/create-a2.md!}
| Instance Type | Select an instance type. For information about instance types on Azure refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general). |
{!docs/common/create-a3.md!}
| SSH Public Key | Specify a public SSH key. You will use the matching private key to access your cluster nodes via SSH. |

{!docs/common/create-a4.md!}

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

{!docs/common/create-adv-4.md!} 


#### Don't Create Public IP

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to use public IPs for the network. 


#### Don't Create New Firewall Rules

This option is available if you are creating a cluster in an existing network and subnet. Select this option if you don't want to create new firewall rules for the network. 


{!docs/common/create-adv-5.md!}  
[Introduction to Microsoft Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction) (External)  


<div class="next">
<a href="../azure-clusters-access/index.html">Next: Access Cluster</a>
</div>

