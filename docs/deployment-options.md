## Deployment options

### Cloudbreak deployment options  

In general, Cloudbreak offers a quickstart option, as well as a production deployment option:

<a href="../images/cb_deployment-options.png" target="_blank" title="click to enlarge"><img src="../images/cb_deployment-options.png" width="650" title="Cloudbreak deployment options"></a>  

* The **quickstart option** allows you to get started with Cloudbreak quickly, but offers limited flexibility. Use this option for getting started with Cloudbreak. This option is not suitable a production.        
* The **production option** is less automated, but offers more configurability. This option is recommended when your production environments. For more information about this option, refer to [Production deployment option](#production-deployment-option).

#### Deployment option cheatsheet

The following table summarizes the available Cloudbreak deployment options:  

<a href="../images/cb_deployment-cheat.png" target="_blank" title="click to enlarge"><img src="../images/cb_deployment-cheat.png" width="650" title="Cloudbreak deployment options"></a>

The following operating systems are used when launching by using the quickstart option:

<a href="../images/cb_deployment-os.png" target="_blank" title="click to enlarge"><img src="../images/cb_deployment-os.png" width="650" title="Cloudbreak deployment options"></a> 
 

#### Quickstart option for AWS   
  
The quickstart option allows you to instantiate Cloudbreak by using the CloudFormation template. This is the basic deployment option and the easiest to get started with. 

<a href="../images/cb_deployment-aws.png" target="_blank" title="click to enlarge"><img src="../images/cb_deployment-aws.png" width="650" title="Cloudbreak deployment options"></a> 

This option utilizes the following AWS services and provisions the following resources:

| Resource | Description | How it is used by Cloudbreak |
|---|---|---|
| [AWS CloudFormation](https://aws.amazon.com/documentation/cloudformation/) | AWS CloudFormation is used to create and manage a collection of related AWS resources. | Cloudbreak is launched by using a CloudFormation template. | 
| [Amazon EC2](https://aws.amazon.com/documentation/ec2/) |  Amazon EC2 is used to launch a virtual machine for Cloudbreak. Security groups are used to control the inbound and outbound traffic to and from the Cloudbreak instance. | Cloudbreak automatically provisions a new VM that runs Amazon Linux, installs Docker, and launches Cloudbreak. |
| [Amazon VPC](https://aws.amazon.com/documentation/vpc/) |  [Amazon VPC](https://aws.amazon.com/documentation/vpc/) is used to provision your own dedicated virtual network and launch resources into that network. As part of VPC infrastructure, an internet gateway and a route table are provisioned: An internet gateway is used to enable outbound access to the internet from the control plane and the clusters, and a route table is used to connect the subnet to the internet gateway. | Cloudbreak provisions a new VPC and subnet, and launches the Cloudbreak VM within it. |   
|  [AWS IAM](https://aws.amazon.com/documentation/iam/) | AWS Identity & Access Management (IAM) is used to control access to AWS services and resources. | Cloudbreak provisions the CloudbreakRole IAM role (for role-based authentication). |  
| [AWS Lambda](https://aws.amazon.com/documentation/lambda/) | This is a utility service for running code in AWS. | Cloudbreak uses AWS Lambda is for running code when deploying Cloudbreak. |  

To launch Cloudbreak on AWS by using the quickstart option, refer to [Launch Cloubreak from template (AWS)](aws-quick.md).   


#### Quickstart option for Azure     

[Comment]: <> (This info was pulled from https://hortonworks.github.io/cloudbreak-azure-docs/index.html)

The quickstart option allows you to instantiate Cloudbreak by using Azure Resource Manager (ARM) template. 

On Azure, resources are organized by using resource groups. When you launch Cloudbreak, you may either select to use an existing resource group or a new resource group is created. The following Azure resources are provisioned within the selected resource group:

* [Virtual network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) (VNet) securely connects Azure resources to each other. You may either launch Cloudbreak into an existing VPC, or a new VPC is created and added to the resource group.  
* [Network security group](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-vnet-plan-design-arm) (NSG) defines inbound and outbound security rules, which control network traffic flow.  
* [Virtual machine](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/overview?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json) runs Cloudbreak. Based on the ARM template, Azure automatically provisions a new VM that runs CentOS 7, installs Docker, and launches Cloudbreak.   
* [Public IP address](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm) is assigned to your VM so that it can communicate with other Azure resources.  
* [Network interface](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface) (NIC) attached to the VM provides the interconnection between the VM and the underlying software network.  
* [Blob storage container](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction) is created to store Cloudbreak Deployer OS disk's data.  

To launch Cloudbreak on Azure by using the quickstart option, refer to [Launch Cloubreak from template (Azure)](azure-quick.md).

#### Quickstart option for GCP   

Based on the Cloud Deployment Manager template, GCP automatically provisions a new VM that runs CentOS 7, installs Docker, and launches Cloudbreak. 

[Comment]: <> (Add more info)

To launch Cloudbreak on GCP by using the quickstart option, refer to [Launch Cloubreak from template (GCP)](gcp-quick.md).


#### Production deployment option  
  
The option to install Cloudbreak deployer manually on your own VM is available for all cloud providers. 

This option:

 * Allows you to provide your own VM with CentOS 7, RHEL 7, or Oracle Linux 7  
 * Allows you to use your custom virtual network (On Azure this is also possible with the quickstart option)  
 * Requires you to install Docker  
 * Requires you to download the cbd tarball, extract it, and configure Cloudbreak deployer  

 To get started with the production option, refer to the instructions for your cloud platform:
 
 * [Launch on AWS](aws-launch.md)   
 * [Launch on Azure](azure-launch.md)   
 * [Launch on GCP](gcp-launch.md)  
 * [Launch on OpenStack](os-launch.md) 


#### Deployment on OpenStack  

You must launch Cloudbreak on OpenStack manually by installing Cloudbreak deployer on your own VM (the production option). There is no available quickstart option. For production steps, refer to [Launch on OpenStack](os-launch.md).   


### Cluster deployment options

On a basic level, Cloudbreak offers three cluster deployment options:

* Basic cluster deployment with prescriptive options  
* Advanced cluster deployment with customized options  
* [Enterprise cluster deployment](data-lake.md ) with a data lake and attached workload clusters: 

> The data lake deployment option is technical preview.    

<a href="../images/cb_deployment-datalake.png" target="_blank" title="click to enlarge"><img src="../images/cb_deployment-datalake.png" width="650" title="Cluster deployment options"></a>


