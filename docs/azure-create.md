## Create a Cluster on Azure 

{!docs/common/cluster-basic-conf1.md!}
{!docs/common/cluster-basic-conf2.md!}

{!docs/common/cluster-basic-net&blue.md!}

7. On the **Add File System** page, select to use one of the following filesystems:

    * *Local HDFS*: No external storage outside of HDFS will be used
    * *Windows Azure Data Lake Storage*: If you select this option, you must provide *Data Lake Store account name*.  
    > You must configure access control for Cloudbreak's service principal manually after cluster installation. You can obtain the service principal ID from the Cloudbreak UI. For more information, refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-access-control).  
    * *Windows Azure Blob Storage*: If you select this option, you must provide:

        | Parameter | Description |
|---|---|  
| Storage Account Name | Enter your account name. |
| Storage Account Access Key | Enter your access key. |
| Use File System As Default | Select this option if you want to make WASB your default file system, instead of HDFS. |

{!docs/common/cluster-basic-review.md!}


### Advanced Options

{!docs/common/cluster-adv-1.md!}
| **Enable availability sets** | Azure implements the concept of [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/infrastructure-availability-sets-guidelines#availability-sets) to support fault tolerance for VM's. You can enable availability sets by using the checkbox, and then add the desired availability sets by providing a name and the desired fault domain count (2 or 3). Next, these defined availability sets can be assigned to specific host groups in the [Choose Blueprint](#choose-blueprint) tab. For more information about availability sets, refer to [Availability Sets](#availability-sets). |
{!docs/common/cluster-adv-2.md!}

{!docs/common/cluster-adv-3.md!} 

#### Add File System

After selecting the filesystem, you can optionally configure the following advanced parameters:

| Parameter | Description |
|---|---|
| Attached Storage Type | Select **single storage for all VMs** or **separate storage for every VM**. Selecting single storage means that your whole cluster's OS disks will be placed in one storage account. Using separate storage for every VM will deploy as many storage accounts as the number of nodes in your cluster, avoiding the IOPS limit of a particular storage account. |
| Persistent Storage Name | Enter a name for the persistent storage directory. Default is **cbstore**. |

{!docs/common/cluster-adv-4.md!}


#### Availability Sets

To support fault tolerance for VMs, Azure introduced the concept of [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability). This allows two or more VMs to be mapped to multiple fault domains, each of which defines a group of virtual machines that share a common power source and a network switch. When adding VMs to an availability set, Azure automatically assigns each VM a fault domain. This SLA includes guarantees that during OS Patching in Azure or during maintenance operations, at least one VM belonging to a given fault domain will be available.

In Cloudbreak UI, availability sets can be configured during cluster creation:

1. Enable availability sets using the checkbox on the **Configure Cluster** page. 

2. Add the desired availability sets by providing a name and the desired fault domain count (2 or 3).

3. The sets defined here can be assigned to the host groups on the **Choose Blueprint** page. One availability set can be assigned to only one host group, so you should define in advance as many availability sets as needed for your host groups. The assignment of fault domains is automated by Azure, so there is no option for this in Cloudbreak UI.

    > **IMPORTANT**: The availability sets should only be used when there is a group of two or more application-tier VMs. Single instances placed in an availability set are not subject to Azure’s SLA, and you will not receive warnings of planned maintenance events. 

4. After the deployment is finished, you can check the layout of the VMs inside an availability set on Azure Portal. You will find the "Availability set" resources corresponding to the host groups inside the deployment's resource group. 

<div class="next">
<a href="../azure-data/index.html">Next: Access Cloud Data</a>
</div>