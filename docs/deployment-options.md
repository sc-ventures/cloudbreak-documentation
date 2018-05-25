## Deployment options 

In general, Cloudbreak offers a quickstart option for AWS, Azure, and GCP cloud platforms, as well as an advanced deployment option (similar for all cloud platforms). 

We recommend that you review the quickstart option for your chosen cloud platform to determine if it fulfills your requirements; If it doesn't, use the advanced deployment option. 
   

### Quickstart option for AWS 

There are two basic deployment options on AWS: 

* **Quickstart option**: Recommended for getting started with Cloudbreak on AWS.    
* **Advanced option**: Install Cloudbreak deployer on your own VM. This option is recommended when your production environment has custom requirements (existing VPC, custom VM requirements, and so on). For more information about this option, refer to [Advanced deployment option](#advanced-deployment-option).   

[Comment]: <> (This info is based on https://docs.hortonworks.com/HDPDocuments/HDCloudAWS/HDCloudAWS-1.16.5/bk_hdcloud-aws/content/index.html) 
  
The quickstart option allows you to instantiate Cloudbreak by using the CloudFormation template. This is the basic deployment option and the easiest to get started with. This option:

* Uses [AWS CloudFormation](https://aws.amazon.com/documentation/cloudformation/) to create and manage a collection of related AWS resources. Cloudbreak is launched by using a CloudFormation template.  

* Provisions a new VM and launches Cloudbreak on it. [Amazon EC2](https://aws.amazon.com/documentation/ec2/) is used to launch a virtual machine for Cloudbreak. Security groups are used to control the inbound and outbound traffic to and from the Cloudbreak instance. 
        
* Provisions a new VPC and subnet, and launches the Cloudbreak VM within it. [Amazon VPC](https://aws.amazon.com/documentation/vpc/) is used to provision your own dedicated virtual network and launch resources into that network. As part of VPC infrastructure, an internet gateway and a route table are provisioned: An internet gateway is used to enable outbound access to the internet from the control plane and the clusters, and a route table is used to connect the subnet to the internet gateway. For more information on Amazon VPC architecture, refer to [Amazon VPC documentation](https://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/ExerciseOverview.html).   

    > The quickstart option always launches Cloudbreak in a new VPC. If you would like to use an existing VPC, you must use the advanced option.  
    
* Allows you to attach an existing IAM role (for role-based authentication). [AWS Identity & Access Management](https://aws.amazon.com/documentation/iam/) is used to control access to AWS services and resources. Cloudbreak does not create any IAM roles but allows you to use exiting IAM roles.   
* [AWS Lambda](https://aws.amazon.com/documentation/lambda/), a utility service for running code in AWS, is used when deploying Cloudbreak.  

[Comment]: <> (What exactly is this Lambda service used for?) 

To launch Cloudbreak on AWS by using the quickstart option, refer to [Launch Cloubreak from template (AWS)](aws-launch.md).   


### Quickstart option for Azure  

There are two basic deployment options on Azure:

* **Quickstart option**: Instantiate Cloudbreak by using Azure Resource Manager template. This is the basic deployment option and the easiest to get started with. It allows you to either use an existing VCP or launch Cloudbreak into a new VPC.   
* **Advanced option**: Install Cloudbreak deployer on your own VM. This option is recommended if you have custom VM requirements. For more information about this option, refer to [Advanced deployment option](#advanced-deployment-option).   

[Comment]: <> (This info was pulled from https://hortonworks.github.io/cloudbreak-azure-docs/index.html)

The quickstart option allows you to instantiate Cloudbreak by using Azure Resource Manager template. 

On Azure, resources are organized by using resource groups. When you launch Cloudbreak, you may either select to use an existing resource group or a new resource group is created. The following Azure resources are provisioned within the selected resource group:

* [Virtual network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) (VNet) securely connects Azure resources to each other. You may either launch Cloudbreak into an existing VPC, or a new VPC is created and added to the resource group.  
* [Network security group](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-vnet-plan-design-arm) (NSG) defines inbound and outbound security rules, which control network traffic flow.  
* [Virtual machine](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/overview?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json) runs Cloudbreak.  
* [Public IP address](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm) is assigned to your VM so that it can communicate with other Azure resources.  
* [Network interface](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface) (NIC) attached to the VM provides the interconnection between the VM and the underlying software network.  
* [Blob storage container](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction) is created to store Cloudbreak Deployer OS disk's data.  

To launch Cloudbreak on Azure by using the quickstart option, refer to [Launch Cloubreak from template (Azure)](azure-launch.md).

### Quickstart option for GCP   

There are two basic deployment options:

* **Quickstart option**: Instantiate Cloudbreak by using Google's Cloud Deployment Manager. This is the basic deployment option and the easiest to get started with.   
* **Advanced option**: Install Cloudbreak deployer on your own VM. This option is recommended when your production environment has custom requirements (existing VPC, custom VM requirements, and so on). For more information about this option, refer to [Advanced deployment option](#advanced-deployment-option). 

MORE INFO - TBD 

To launch Cloudbreak on GCP by using the quickstart option, refer to [Launch Cloubreak from template (GCP)](gcp-launch.md).


### Deployment on OpenStack  

You must launch Cloudbreak on OpenStack manually by installing Cloudbreak deployer on your own VM. There is no available quickstart option. Refer to [Launch on OpenStack](os-launch.md). 


### Advanced deployment option  
  
The option to install Cloudbreak deployer manually on your own VM is available for all cloud providers. Use this option if the template-based deployment does not fulfill your requrements.

To get started, refer to [Launch Cloudbreak on your own VM](vm-launch.md).


