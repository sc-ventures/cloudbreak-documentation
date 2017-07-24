## Define Infrastructure Templates and Blueprints 

After you've logged in to Cloudbreak and created a Cloudbreak credential, you must:

* Define infrastructure templates for certain cluster resources  
* Define cluster blueprints or reuse default bluepritnts  

The **infrastructure templates** for resources such as **networks**, **security groups**, and **VMs and storage** are saved to Cloudbreak's database and can be reused with multiple clusters to describe the infrastructure. When you add these resources in Cloudbreak web UI, Cloudbreak does not make any requests to your cloud provider account. Resources are only created on your cloud provider account after the create cluster button has been pushed. 

Similarly, **blueprints** are saved to Cloudbreak's database and can be reused with multiple clusters to describe the cluster components.

This is illustrated in the following image: 

<a href="../images/templates-and-blueprints.png" target="_blank" title="click to enlarge"><img src="../images/templates-and-blueprints.png" width="550" title="How Cloudbreak uses templates and blueprints"></a> 

You can add infrastructure templates and blueprints after expanding their corresponding panes in the Cloudbreak web UI (shown in the screenshot) as described in the documentation below.  

<a href="../images/cb-reusable-configs.png" target="_blank" title="click to enlarge"><img src="../images/cb-reusable-configs.png" width="650" title="Azure Portal"></a> 


The following table describes the basic configurations for which you must create custom tempalates:


| Configuration | Description |  Create Cluster | Preconfigured |
|---|---|---|---
| [Networks](#networks) |(Required) Virtual networks provide the networking infrastructure (network, subnet, Internet gateway, and so on) in which your clusters run. You can create new virtual networks or reuse existing virtual networks for your clusters. | You can select the network configuration for your clusters in the **Create Cluster** wizard > **Set up Network and Security** page. | No |
| [Security Groups](#security-groups) | (Required) Security groups include rules which define inbound traffic allowed to the instances in your cluster. You can define different security group configurations for different nodes of your cluster. | You can select the security group configurations for each host group in the **Create Cluster** wizard > **Choose Blueprint** page. If no custom security groups are selected, default is used. | Yes | 
| [VMs and Storage](#vms-and-storage) | (Required) "Templates" define the infrastructure for your clusters: the instances on which your clusters run and their attached storage type, size, and count. You can reuse the same template for different cluster host groups or create different templates for different host groups. | You can select infrastructure templates for each host group in the **Create Cluster** wizard > **Choose Blueprint** page. | No |
| [Blueprints](#blueprints) | (Required) Blueprints are your declarative definition of a Hadoop cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. | You can select a blueprint for your cluster in the **Create Cluster** wizard > **Choose Blueprint** page. A few default blueprints are available. | Yes | 

The "Preconfigured" column indicates whether a default template is available, indicating that templates are available for security groups and blueprints. 


### Networks

Before you can start using Cloubreak, you must define at least one custom network that can be used for cluster instances.

An instance uses a public provider virtual network that connects to the physical network infrastructure. This network includes a DHCP server that provides IP addresses to instances. The admin or other privileged user must create this network because it connects directly to the physical network infrastructure. Learn more about OpenStack [virtual network](https://docs.openstack.org/liberty/install-guide-rdo/launch-instance.html#create-virtual-networks) and [public provider network](https://docs.openstack.org/liberty/install-guide-rdo/launch-instance-networks-public.html).

Your clusters can be created in new networks or in one of your existing networks. If you choose an existing network, it is possible to create a new subnet within the network. 

#### Add New Network and Subnet

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Create a new virtual network and a new subnet**.

2. Provide required parameters:

    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Subnet (CIDR) | A valid CIDR notation e.g. 10.0.0.0/24. Define the IP address space usable by your VMs within the cluster. Cloudbreak supports the private address space defined in [RFC1918](https://tools.ietf.org/html/rfc1918). |
| Floating Pool ID |  Floating IPs are not automatically allocated to instances by default, they needs to be attached to instances after creation. The Floating IPs are used to provide access to your VMs running on OpenStack. You can figure out the available network pools and their IDs by using the nova floating-ip-pool-list and neutron net-external-list or copy-pasting it from OpenStack Horizon UI. Such networks have the External Network field set to Yes. If you are unable to find the ID then just consult with your OpenStack network administrator. Please note that if you do not set this field then your cluster might not be accessible. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.


#### Add New Subnet in an Existing Network

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Create a new subnet in an existing virtual network**.

2. Provide required parameters:

    > Make sure that the new subnet defined here doesn't overlap with any of your already deployed subnets in the network. 


    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Subnet (CIDR) | A valid CIDR notation e.g. 10.0.0.0/24 |
| Floating Pool ID | Floating IPs are not automatically allocated to instances by default, they needs to be attached to instances after creation. The Floating IPs are used to provide access to your VMs running on OpenStack. You can figure out the available network pools and their IDs by using the nova floating-ip-pool-list and neutron net-external-list or copy-pasting it from OpenStack Horizon UI. Such networks have the External Network field set to Yes. If you are unable to find the ID then just consult with your OpenStack network administrator. Please note that if you do not set this field then your cluster might not be accessible. |
| Virtual Network Identifier | This is the ID of an existing virtual network on OpenStack where you would like to launch the cluster. |
| Router Identifier | Specify the router ID that shall interconnect your existing Network with the Subnet which will be created by CLoudbreak. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |


3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.


#### Use Existing Network and Subnet

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Use an existing subnet in an existing virtual network**. 

2. Provide required parameters: 

    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Floating Pool ID | Floating IPs are not automatically allocated to instances by default, they needs to be attached to instances after creation. The Floating IPs are used to provide access to your VMs running on OpenStack. You can figure out the available network pools and their IDs by using the nova floating-ip-pool-list and neutron net-external-list or copy-pasting it from OpenStack Horizon UI. Such networks have the External Network field set to Yes. If you are unable to find the ID then just consult with your OpenStack network administrator. Please note that if you do not set this field then your cluster might not be accessible. |
| Virtual Network Identifier | This is the ID of an existing virtual network on OpenStack where you would like to launch the cluster. |
| Subnet Identifier | This is the ID of an existing subnet on OpenStack where you would like to launch the cluster.  |
| Networking Option | |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

    > When using an existing subnet, make sure that you have enough room within your network space for the new instances. The provided subnet CIDR will be ignored, but a proper CIDR range will be used.

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.
    
    

### Security Groups

You have two options:

* **Use the default security group configuration**: This requires no further action.  
* **Add your own custom security group configuration**: You can define your own security group by adding the ports, protocols, and CIDR ranges that you'd like to use. The rules defined here don't need to contain the internal rules (these are automatically added by Cloudbreak to the security group on OpenStack). 

#### Default Security Group

Cloudbreak includes one pre-defined security group configuration called *default-openstack-only-ssh-and-ssl*, which is used by default when creating a cluster. You can see it in the **manage security groups** tab. The configuration is: 

{!docs/common/config-sg.md!}     
    


### VMs and Storage 

Before you can start using CLoubreak, you must define at least one custom template defining the OpenStack infrastructure that will be used for cluster nodes and storage.  

#### Add Custom Template

You can define reusable cluster templates in the **manage templates** tab: 

1. Click on **+create template**.

2. Provide required parameters: 

    | Parameter | Description |
|---|---|
| Instance Type | Select an instance type. The instance types available depend on your OpenStack implementation. |
| Attached Volumes Per Instance | |
| Volume Size (GB) | 10 |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create template**.

    No resources will be created until you create a cluster using this configuration.
 

### Blueprints 

{!docs/common/blueprints.md!}  



<div class="next">
<a href="../os-create/index.html">Next: Create a Cluster</a>
</div>