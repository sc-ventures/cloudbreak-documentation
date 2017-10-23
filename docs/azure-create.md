## Create a Cluster on Azure 

### Optional Prerequisites 

If you are just getting started with Cloudbreak, follow the steps below to create a cluster using the default blueprints. 

If you are already familiar with Cloudbreak and you want to use custom blueprints, refer to the [Blueprints](blueprints.md) documentation to customize your blueprints.

### Basic Options 

Use these steps to create a cluster.

**Steps**

1. Log in to the Cloudbreak UI.

3. Click **Create Cluster** and the *Create Cluster* form is displayed.

4. On the **General Configuration** page, provide the following parameters:

    > To view advanced options, click **Advanced**. To learn about advanced options, refer to [Advanced Options](#advanced-options).

    | Parameter | Description |
|---|---|
| Select Credential | Select a previously created credential. |
| Cluster Name | Enter a name for your cluster. The name must be between 5 and 40 characters, must start with a letter, must only include lowercase letters, numbers, and hyphens. |
| Tags | (Optional) You can optionally add tags, which will help you find your cluster-related resources, such as VMs, in your cloud provider account. |
| Region | Select the region in which you would like to launch your cluster. |
| Send Email When Cluster is Ready | (Optional) Check this to receive an email each time the cluster status changes. |

    > By default, Ambari Username and Ambari Password are set to `admin`. You can override it in the "[Configure Cluster](#configure-cluster)" tab.
    
6. On the **Hardware and Storage** page, select the blueprint that you would like to use for your cluster. You can either choose one of the pre-configured blueprints, or add your own in the **Blueprints** tab.

    For each host group you must provide the following:
    
    | Parameter | Description |
|---|---|
| Group Size | Enter a number defining how many nodes to create per host group. Default is 1. The "Group Size" for that host group on which Ambari Server is installed must be set to "1". |
| Template | If you have previously created a template for VMs and storage, you can select it here. If you don't make a selection, default will be used. |
| Security Group | If you have previously created a template for a security group, you can select it here. If you don't make a selection, default will be used. |
| Ambari Server | You must select one node for Ambari Server. The "Group Size" for that host group must be set to "1". |
| Recipes | You can select a previously added recipe (custom script) to be executed on all nodes of the host group. Refer to [Recipes](recipes.md). |    

7. On the **File System** page, select to use one of the following filesystems:

    * *Local HDFS*: No external storage outside of HDFS will be used.
    * *Windows Azure Data Lake Storage*: If you select this option, HDFS will be used as your default file system and access to ADLS will be through the adl connector. You must provide:

        | Parameter | Description |
|---|---|  
| Data Lake Store account name | Enter your account name. |
        
        **Postrequisites**: After cluster installation, you must perform additional steps described in [Configuring Access to ADLS](azure-data.md#configuring-access-to-adls). 
        
    * *Windows Azure Blob Storage*: If you select this option, HDFS will be used as your default file system and access to WASB will be through the wasb connector, unless you select **Use File System As Default**. You must provide:

        | Parameter | Description |
|---|---|  
| Storage Account Name | Enter your account name. |
| Storage Account Access Key | Enter your access key. |
| Use File System As Default | Select this option if you want to make WASB your default file system, instead of HDFS. |

5. On the **Network** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Network | Select the virtual network in which you would like your cluster to be provisioned. You can define custom network configurations or use default network configurations. |
| Enable Knox Gateway | (Optional) Select this option to enable secure access to Ambari web UI and other cluster UIs via Knox gateway. |
| Enable Kerberos Security | (Optional) Select this option to enable Kerberos for your cluster. You will have an option to create a new kerberos or use an existing one. For more information refer to Kerberos [documentation](http://sequenceiq.com/cloudbreak-docs/latest/kerberos/). |

5. On the **Security** page, provide the following parameters:

8. Click on **Create Cluster** to create a cluster.

9. You will be redirected to the Cloudbreak dashboard, and a new tile representing your cluster will appear at the top of the page.


### Advanced Options

Click on **Show Advanced Options** to enter additional configuration options.

#### General Configuration

You can optionally configure the following advanced parameters:

| Parameter | Description |
|---|---|
| Ambari Username | You can log in to the Ambari UI using this username. By default, this is set to `admin`. |
| Ambari Password | You can log in to the Ambari UI using this password. By default, this is set to `admin`. |
| Provision Cluster | **SALT** is pre-selected to provision your cluster. |
| **Enable availability sets** | Azure implements the concept of [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/infrastructure-availability-sets-guidelines#availability-sets) to support fault tolerance for VM's. You can enable availability sets by using the checkbox, and then add the desired availability sets by providing a name and the desired fault domain count (2 or 3). Next, these defined availability sets can be assigned to specific host groups in the [Choose Blueprint](#choose-blueprint) tab. For more information about availability sets, refer to [Availability Sets](#availability-sets). |
| Enable Lifetime Management | Check this option if you would like your cluster to be automatically terminated after a specific amount of time (defined as "Time to Live" in minuter) has passed. |
| Flex Subscription | This option will appear if you have configured your deployment for a [Flex Subscription](get-help.md#flex-subscription). |

#### Hardware and Storage

After selecting a blueprint, you can optionally configure the following advanced parameters:

| Parameter | Description |
|---|---|
| Config Recommendation Strategy (Stack Advisor) | Select how configuration recommendations generated by stack advisor will be applied. Select one of <p><ul><li>ALWAYS_APPLY: Configuration recommendations will be applied automatically.</li><li>ALWAYS_APPLY_DONT_OVERRIDE_CUSTOM_VALUES: Configuration recommendations will be applied automatically, but they will be ignored for custom configurations.</li><li>NEVER_APPLY: Configuration recommendations will be ignored.</li><li>ONLY_STACK_DEFAULTS_APPLY: Configuration recommendations will be applied only on the default configurations for all included services.</li></ul></p> |
| Validate Blueprint | Select to validate the blueprint. |

#### Add File System

After selecting the filesystem, you can optionally configure the following advanced parameters:

| Parameter | Description |
|---|---|
| Attached Storage Type | Select **single storage for all VMs** or **separate storage for every VM**. Selecting single storage means that your whole cluster's OS disks will be placed in one storage account. Using separate storage for every VM will deploy as many storage accounts as the number of nodes in your cluster, avoiding the IOPS limit of a particular storage account. |
| Persistent Storage Name | Enter a name for the persistent storage directory. Default is **cbstore**. |


#### Choose Failure Action

You can optionally select what to do if cluster creation fails or if there aren't enough instances available to create all requested nodes:

| Parameter | Description |
|---|---|
| Failure Action | <p>Select one of: **do NOT rollback resources** (default) or **rollback resources**. </p><p>By default, if creating a cluster fails, the Azure resources that were created up to that point will not be rolled back. This means that they will remain accessible for troubleshooting and you will need to to delete them manually.</p> |
| Minimum Cluster Size | This defines the provisioning strategy in case the cloud provider cannot allocate all the requested nodes. Select **best effort** or **exact**.  |


#### Configure Ambari Repos

You can optionally configure a different version of Ambari than the default by providing the following information:

| Parameter | Description |
|---|---|
| Ambari Version| Enter Ambari version. |
| Ambari Repo URL | Enter Ambari repo URL. |
| Ambari Repo Gpg Key URL | Enter gpgkey URL. |


#### Configure HDP Repos

You can optionally configure a different version of HDP than the default by providing the following information:

| Parameter | Description |
|---|---|
| Stack | Enter stack name. |
| Version | Enter stack version. |
| Stack Repo ID | Enter stack repo ID. |
| Base URL| Ener stack repo base URL. |
| Utils Repo ID | Enter Utils repo ID. |
| Utils Base URL | Enter Utils repo base URL. |
| Verify | Select to verify the repo information. |


#### Configure Ambari Database

By default, Ambari stores data on an embedded database, which is sufficient for ephemeral or test clusters. However, as Ambari and Cloudbreak don't perform backups of this database, it is insufficient for long-running production clusters, and you may need to configure a remote database for Ambari and Cloudbreak.

| Parameter | Description |
|---|---|
| Vendor | Select database vendor from the list. |
| Host | Enter database host IP. |
| Port | Enter port number. |
| Name | Enter database name. |
| User Name | Enter database user name. |
| Password | Enter database password. |


#### Availability Sets

To support fault tolerance for VMs, Azure introduced the concept of [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability). This allows two or more VMs to be mapped to multiple fault domains, each of which defines a group of virtual machines that share a common power source and a network switch. When adding VMs to an availability set, Azure automatically assigns each VM a fault domain. This SLA includes guarantees that during OS Patching in Azure or during maintenance operations, at least one VM belonging to a given fault domain will be available.

In Cloudbreak UI, availability sets can be configured during cluster creation:

1. Enable availability sets using the checkbox on the **Configure Cluster** page. 

2. Add the desired availability sets by providing a name and the desired fault domain count (2 or 3).

3. The sets defined here can be assigned to the host groups on the **Choose Blueprint** page. One availability set can be assigned to only one host group, so you should define in advance as many availability sets as needed for your host groups. The assignment of fault domains is automated by Azure, so there is no option for this in Cloudbreak UI.

    > **IMPORTANT**: The availability sets should only be used when there is a group of two or more application-tier VMs. Single instances placed in an availability set are not subject to Azure’s SLA, and you will not receive warnings of planned maintenance events. 

4. After the deployment is finished, you can check the layout of the VMs inside an availability set on Azure Portal. You will find the "Availability set" resources corresponding to the host groups inside the deployment's resource group. 


<div class="next">
<a href="../azure-clusters-access/index.html">Next: Access Cluster</a>
</div>
