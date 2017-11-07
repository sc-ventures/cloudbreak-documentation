## Security Overview

To ensure maximum security for your clusters, Cloudbreak utilizes cloud provider security resources such as virtual networks, security groups, and identity and access management. In addition, Coudbreak allows you to configure Kerberos to ensure secure authentication.

1. **Network isolation** is achieved via user-configured virtual networks and subnets.  
    Read more about [Virtual Networks](#virtual-networks).  
2. **Network security** is achieved via out-of-the-box security group settings.  
    Read more about [Network Security](#network-security).   
3. **Controlled use of cloud resources** using IAM roles (AWS, GCP) or Active Directory (in case of Azure). 
    Read more about [Identity Management](#identity-management).    
4. **Authentication** using Kerberos. Read more about [Kerberos](#kerberos).   

### Virtual Networks

Cloud providers use virtual networks which resemble traditional networks. Depending on the options that you selected during deployment, your Cloudbreak instance and clusters are launched into new or existing cloud provider networking infrastructure (virtual networks and subnets). For more information about virtual networks, refer to the cloud-provider documentation:
  
| Cloud Provider | Documentation Link |
|---|---|
| AWS | [Virtual Private Cloud (VPC)](https://aws.amazon.com/documentation/vpc/) |
| Azure | [Virtual network (VNet)](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) |
| Google Cloud Platform | [VPC network](https://cloud.google.com/compute/docs/vpc/) |
| OpenStack | [Network](https://docs.openstack.org/mitaka/networking-guide/intro-os-networking.html) |

### Network Security 

Security groups are set up to control network traffic to the instances in the system. By default, the system is configured to restrict inbound network traffic to the minimal set of ports. You can add or modify rules to each security group that allow traffic to or from its associated instances: you can do this either when creating your cluster or when the cluster is already running.  

The following table lists the minimum security group port configuration required for the Cloudbreak instance:

| Inbound Port | Description |
|---|---|
| 22 | SSH access to the Cloudbreak VM. |
| 80 | HTTP access to the Cloudbreak UI. |

[comment]: <> (How about cluster security groups? I see plenty of ports open on master and worker security groups.)


### Identity Management

To securely control access to cloud resources, cloud providers use identity management services such as IAM roles (AWS and GCP) and Active Directory (Azure). 

| Cloud Provider | Documentation Link | How is IAM used by Cloudbreak |
|---|---|---| 
| AWS | IAM [documentation](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) | When launching Cloudbreak on AWS, you can select to use key-based or role-based authenticaton. While key-based authentication uses your Access key and Secret key, role-based authentication uses IAM roles. |
| Azure | Active Directory [documentation](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) | After launching Cloudbread on Azure, you are required to create a Cloudbreak credential, which uses your Azure Active Directory to authenticate with the Azure cloud provider. |
| Google | IAM [documentation](https://cloud.google.com/iam/docs/overview) | When launching Cloudbreak on GCP, you are required to create a service account that Cloudbreak can use to authenticate with your account. |


### Kerberos

Cloudbreak supports using Kerberos security for its managed clusters. It supports three ways of provisioning Kerberos-enabled clusters:

* Create new MIT Kerberos at cluster provisioning time  
* Use your existing MIT Kerberos server with a Cloudbreak provisioned cluster  
* Use your existing Active Directory with a Cloudbreak provisioned cluster  

For more information about Kerberos, refer to [Configure Kerberos](security-kerberos.md) documentation.

