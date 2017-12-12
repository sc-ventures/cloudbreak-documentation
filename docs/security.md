## Security Overview

Cloudbreak utilizes cloud provider security resources such as virtual networks, security groups, and identity and access management:

1. **Network isolation** is achieved via user-configured virtual networks and subnets.  
    Read more about [Virtual Networks](#virtual-networks).  
2. **Network security** is achieved via out-of-the-box security group settings.  
    Read more about [Network Security](#network-security).   
3. **Controlled use of cloud resources** using IAM roles (AWS, GCP) or Active Directory (in case of Azure). 
    Read more about [Identity Management](#identity-management).    
 

### Virtual Networks

Cloud providers use virtual networks which resemble traditional networks. Depending on the options that you selected during deployment, your Cloudbreak instance and clusters are launched into new or existing cloud provider networking infrastructure (virtual networks and subnets). For more information about virtual networks, refer to the cloud-provider documentation:
  
| Cloud Provider | Documentation Link |
|---|---|
| AWS | [Amazon Virtual Private Cloud (Amazon VPC)](https://aws.amazon.com/documentation/vpc/) |
| Azure | [Microsoft Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) |
| Google Cloud Platform | [Virtual Private Cloud (VPC) network](https://cloud.google.com/compute/docs/vpc/) |
| OpenStack | [Network](https://docs.openstack.org/mitaka/networking-guide/intro-os-networking.html) |

### Network Security 

Security groups are setup to control network traffic to the instances in the system.

#### Cloudbreak Instance Security Group

The following table lists the minimum security group port configuration required for the Cloudbreak instance:

| Inbound Port | Description |
|---|---|
| 22 | SSH access to the Cloudbreak VM. |
| 80 | HTTP access to the Cloudbreak UI. This is automatically redirected to the HTTPS (443) port. |
| 443 | HTTPS access to the Cloudbreak UI. |

#### Default Cluster Security Groups 

For clusters, Cloudbreak provides the following security group settings. If you do not modify these settings, these default security rules will be created. You can modify these rules either when creating or, if you don't want to use security group, remove them. 

As an alternative to creating new security groups, you can select from your existing set of security groups, which can be modified using the cloud provider tools. 
 
**Cluster Host Group with Ambari Server**

| Inbound Port | Description |
|---|---|
| 22 | SSH access to the VM instance. |
| 443 | HTTPS access to the Ambari UI. |
| 9443 | Management port, used by Cloudbreak to communicate with the cluster node VM. |

<div class="danger">
    <p class="first admonition-title">Important</p>
    <p class="last">
By default, ports 22, 443, and 9443 are set to 0.0.0.0/0 CIDR for inbound access. We strongly recommend that you limit this CIDR in the security group:
<ul><li>For port 22 to only allow traffic from your public IP and from your Cloudbreak VM instance public IP.</li>
<li>For port 9443 to only allow traffic from your Cloudbreak VM instance public IP.</li> 
<li>For port 443 to only allow traffic from your public IP and from your Cloudbreak VM instance public IP.</li></ul>
You can set cb.default.gateway.cidr=Cloudbreak_IP in your Cloudbreak's Profile file in order to automatically open ports 9443 and 22 to your Cloudbrak IP.  
</p>
</div>

**Cluster Host Groups without the Ambari Server**

| Inbound Port | Description |
|---|---|
| 22 | SSH access to the VM instance. |

<div class="danger">
    <p class="first admonition-title">Important</p>
    <p class="last">
By default, port 22 is set to 0.0.0.0/0 CIDR for inbound access. We strongly recommend that you remove it.</p>
</div>

When creating a new security group, Cloudbreak uses the following naming convention: `<clustername>-ClusterNodeSecurityGroup<hostgroupname>` 


### Identity Management

To securely control access to cloud resources, cloud providers use identity management services such as IAM roles (AWS and GCP) and Active Directory (Azure). 

| Cloud Provider | Documentation Link | 
|---|---|
| AWS | [AWS Identity and Access Management (IAM)](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) |
| Azure | [Azure Active Directory ((Azure AD))](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) | 
| Google | [Google Cloud Identity and Access Management (IAM)](https://cloud.google.com/iam/docs/overview) | 
| OpenStack | [Keystone](https://docs.openstack.org/keystone/pike/) |

Cloudbreak utilizes cloud provider’s identity management services via Cloudbreak credential. After launching Cloudbreak on your chosen cloud provider, you must create a Cloudbreak credential, which allows Cloudbreak to authenticate with your cloud provider identity management service. Only after you have completed this step, Cloudbreak can create resources on your behalf. 


#### Authentication with AWS

When launching Cloudbreak on AWS, you must select a way for Cloudbreak to authenticate with your AWS account and create resources on your behalf. While key-based authentication uses your AWS access key and secret key, role-based authentication uses IAM roles.

{!docs/common/aws-launch-authentication-role-based-intro.md!}

**Related Links**  
[Meet the Prerequisites: Authentication](aws-launch.md#authentication)  


#### Authentication with Azure

After launching Cloudbread on Azure, you are required to create a Cloudbreak credential, which allows Cloudbreak to authenticate with your Azure Active Directory. 

You have two options:

* Interactive: The app and service principal creation and role assignment are fully automated, so the only input that you need to provide to Cloudbreak is your Subscription ID and Directory ID. 

* App-based: The app and service principal creation and role assignment are not automated You must create an Azure Active Directory application registration and then provide its parameters to Cloudbreak, in addition to providing your Subscription ID and Directory ID. 

**Related Links**  
[Create Cloudbreak Credential](azure-launch.md#create-cloudbreak-credential)  


#### Authentication with GCP

After launching Cloudbreak on GCP, you are required to register a service account in Cloudbrak via creating a Cloudbreak credential. Cloudbreak uses this account to authenticate with the GCP identity management service.

**Related Links**  
[Meet the Prerequisites: Service Account](gcp-launch.md#service-account)  


#### Authentication with OpenStack 

After launching Cloudbread on OpenStack, you are required to create a Cloudbreak credential, which allows Cloudbreak to authenticate with keystone. 


**Related Links**  
[Create Cloudbreak Credential](os-launch.md#create-cloudbreak-credential)  
