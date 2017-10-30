## Create a Cluster on GCP 

Use these steps to create a cluster.

**Steps**

1. Log in to the Cloudbreak UI.

2. Click **Create Cluster** and the *Create Cluster* form is displayed.

    To view advanced options, click **Advanced**. To learn about advanced options, refer to [Advanced Options](#advanced-options).

    <a href="../images/cb-create.png" target="_blank" title="click to enlarge"><img src="../images/cb-create.png" width="650" title="Cluster Information"></a> 

3. On the **General Configuration** page, specify the following general parameters for your cluster:

    | Parameter | Description |
|---|---|
| Select Credential | Choose a previously created credential. |
| Cluster Name | Enter a name for your cluster. The name must be between 5 and 40 characters, must start with a letter, and must only include lowercase letters, numbers, and hyphens. |
| Region | Select the region in which you would like to launch your cluster. |
| Platform Version | Choose the HDP version to use for this cluster. |
| Cluster Type | Choose one of default cluster configurations, or, if you have defined your own cluster configuration via Ambari blueprint, you can choose it here. For more information, refer to [Blueprints](blueprints.md). |

    
4. On the **Hardware and Storage** page, for each host group provide the following information to define your cluster nodes and attached storage:
    
    | Parameter | Description |
|---|---|
| Instance Type | Select a VM instance type. For more information about instance types on Azure refer to [GCP documentation](https://cloud.google.com/compute/docs/machine-types).|
| Instance Count | Enter the number of instances of a given type. Default is 1. |
| Ambari Server | You must select one node for Ambari Server. The "Group Size" for that host group must be set to "1". | 

6. On the **Network** page, provide the following to specify the networking resources that will be used for your cluster:

    | Parameter | Description |
|---|---|
| Select Network | Select the virtual network in which you would like your cluster to be provisioned. You can select an existing network or create a new network. |
| Select Subnet | Select the subnet in which you would like your cluster to be provisioned. You must create a new subnet. |
| Subnet (CIDR)| If you selected to create a new subnet, you must define a valid [CIDR](http://www.ipaddressguide.com/cidr) for the subnet. Default is 10.0.0.0/16. |
   
5. On the **Security** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Cluster User | You can log in to the Ambari UI using this username. By default, this is set to `admin`. |
| Password | You can log in to the Ambari UI using this password. |
| Confirm Password | Confirm the password. |
| SSH Key Pair| Select an existing public key or specify a new public key. You will use the matching private key to access your cluster nodes via SSH. |

8. Click on **Create Cluster** to create a cluster.

9. You will be redirected to the Cloudbreak dashboard, and a new tile representing your cluster will appear at the top of the page.



### Advanced Options

Click on **Advanced** to view and enter additional configuration options.


#### Availability Zone

 Choose one of the availability zones within the selected region. 
 
#### Enable Lifetime Management 

Check this option if you would like your cluster to be automatically terminated after a specific amount of time (defined as "Time to Live" in minutes) has passed. 

#### Tags

You can optionally add tags, which will help you find your cluster-related resources, such as VMs, in your cloud provider account. refer to [Resource Tagging](tags.md).


#### Storage

You can optionally specify the following storage options for your cluster:

| Parameter | Description |
|---|---|
| Storage Type |  <p>Select the volume type. The options are:<ul><li>Standard persistent disks (HDD)<li><li>Solid-state persistent disks (SSD)<li></ul> For more information about these options refer to <a href="https://cloud.google.com/compute/docs/disks/" target="_blank">GCP documentation</a>. |
| Attached Volumes Per Instance | Enter the number of volumes attached per instance. Default is 1. |
| Volume Size (GB) | Enter the size in GBs for each volume. Default is 100. |  


#### Recipes

This option allows you to select previously uploaded recipes (scripts that will be run pre- or post- cluster deployment) for each host group. For more information on default and custom blueprints, refer to [Recipes](recipes.md).


#### Security Groups 

For each host group, select one of the options:

* Create new security group  
* Do not use security group  
* Select an existing security group  

If you choose to create a new security group, the *New Security Group* wizard will open.
   
1. TCP ports 22, 443, and 9443 will be open by default. These ports must be open on every security group; otherwise Cloudbreak will not be able to communicate with your provisioned cluster.  
2. You may open additional ports by defining the **CIDR**, **Port**, and **Protocol** for each and clicking **Add Rule**. 
3. Once done, click **SAVE** to save your security group settings.
4. Once you define a custom security group for one host group, you can reuse this definition for other node groups.


#### Enable Kerberos Security

Select this option to enable Kerberos for your cluster. You will have an option to create a new kerberos or use an existing one. For more information refer to [Kerberos](security-kerberos.md) documentation. 


<div class="next">
<a href="../gcp-clusters-access/index.html">Next: Access Cluster</a>
</div>


