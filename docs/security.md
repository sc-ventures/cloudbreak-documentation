## Security overview

Cloudbreak utilizes cloud provider security resources such as virtual networks, security groups, and identity and access management:

1. **Network isolation** is achieved via user-configured virtual networks and subnets.  

2. **Network security** is achieved via out-of-the-box security group settings.  
  
3. **Controlled use of cloud resources** using IAM roles (AWS, GCP) or Active Directory (in case of Azure). 


### Virtual networks

Cloud providers use virtual networks which resemble traditional networks. Depending on the options that you selected during deployment, your Cloudbreak instance and clusters are launched into new or existing cloud provider networking infrastructure (virtual networks and subnets). For more information about virtual networks, refer to the cloud-provider documentation:
  
| Cloud provider | External documentation link |
|---|---|
| AWS | [Amazon Virtual Private Cloud (Amazon VPC)](https://aws.amazon.com/documentation/vpc/) |
| Azure | [Microsoft Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) |
| Google Cloud Platform | [Virtual Private Cloud (VPC) network](https://cloud.google.com/compute/docs/vpc/) |
| OpenStack | [Network](https://docs.openstack.org/mitaka/networking-guide/intro-os-networking.html) |


### Network security 

Security groups are set up to control network traffic to the instances in the system.

Cloudbreak uses public IP addresses when communicating with cluster nodes. On AWS, you can configure it to use private IPs instead. For instructions, refer to [Configure communication via private IPs on AWS](trouble-cluster.md#configure-communication-via-private-ips-on-aws).  

**Related links**  
[Configure communication via private IPs on AWS](trouble-cluster.md#configure-communication-via-private-ips-on-aws)

#### Cloudbreak instance security group

The following table lists the minimum security group port configuration required for the Cloudbreak instance:

| Inbound port | Description |
|---|---|
| 22 | SSH access to the Cloudbreak VM. |
| 80 | HTTP access to the Cloudbreak UI. This is automatically redirected to the HTTPS (443) port. |
| 443 | HTTPS access to the Cloudbreak UI. |

#### Default cluster security groups 

The following section describes the network requirements and options. By default, when creating a cluster, a new network, subnet, and security groups will be created automatically.

<div class="note">
    <p class="first admonition-title">Note</p>
    <p class="last">By default, when creating a cluster, a new network, subnet, and security groups are created automatically. The default experience of creating network resources such as network, subnet and security group automatically is provided for convenience. We strongly recommend that you review these options and for production cluster deployments leverage your existing network resources that you have defined and validated to meet your enterprise requirements.</p>
</div>

<div class="note">
    <p class="first admonition-title">Note</p>
    <p class="last">Depending on the cluster components that you are planning to use, you may need to open additional ports required by these components.</p>
</div>

**External ports**

| Source | Target | Protocol | Port | Description |
|---|---|---|---|---|
| Cloudbreak | Ambari server | TCP | 9443  | <ul><li>This port is used by Cloudbreak to maintain management control of the cluster.</li><li>The default security group opens 9443 from anywhere. You should limit this CIDR further to *only allow access from the Cloudbreak host*. This can be done by default by [restricting inbound access](security-cb-inbound.md) from Cloudbreak to cluster.</li><ul>|
| * | All cluster hosts | TCP | 22 | <ul><li>This is an optional port for end user SSH access to the hosts.</li><li>You should review and limit or remove this CIDR access.</li><ul>|
| * | Ambari server | TCP | 8443 | <ul><li>This port is used to access the gateway (if configured).</li><li>You should review and limit this CIDR access.</li><li>If you do not configure the gateway, this port does not need to be opened. If you want access to any cluster resources, you must open those ports explicitly on the security groups for their respective hosts.</li><ul> |
| * | Ambari server | TCP | 443 | <ul><li>This port is used to access Ambari directly.</li><li>If you are configuring the gateway, you should access Ambari through the gateway. You do not need to open this port.</li><li>If you do not configure the gateway, to obtain access to Ambari, you can open this port on the security group for the respective host.</li><ul> |

**Internal ports**

In addition to the ports described above, Cloudbreak uses certain ports for internal communication within the subnet. By default, Cloudbreak opens ports 0-65535 to the subnet's internal CIDR (such as 10.0.0.0/16). Use the following table to limit this CIDR:

| Source | Target | Protocol |  Port | Description |
|---|---|---|---|---|
| Salt-bootstrap | Gateway instance (Ambari server instance) | TCP | 7070 | Salt-bootstrap service launches and configures Saltstack. |
| Salt-master | All hosts in the cluster | TCP | 4505,4506 | Salt-minions connect to the Salt-master(s). |
| Consul server | All hosts in the cluster | TCP,UDP | 	8300,8301 | Consul agents connect to the Consul server. |
| Consul agent (all hosts in the cluster) | All hosts in the cluster | TCP,UDP | 8300,8301 | Consul agents connect to other Consul agents (Gossip protocol). | 
| Prometheus node exporter | Gateway instance (Ambari server instance) | TCP |	9100 | Prometheus server scrapes metrics from the node exporters. |
| Ambari server | All hosts in the cluster | Refer to [Ambari documentation](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/administering-ambari/content/amb_default_network_port_numbers_ambari.html). | Refer to  [Ambari documentation](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/administering-ambari/content/amb_default_network_port_numbers_ambari.html). | Ambari agents connect to the Ambari server. |

When creating data lakes and their attached clusters,, you must also open the following internal port:

| Source | Target | Protocol | Port | Description |
|---|---|---|---|---|
| Data lake cluster | Clusters attached to the data lake | TCP | 6080 | Used for communication between the data lake and attached clusters. |

**Related links**   
[Restrict inbound access](security-cb-inbound.md)  
[Default network port numbers for Ambari](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/administering-ambari/content/amb_default_network_port_numbers_ambari.html) (Ambari)    


### Identity management

To securely control access to cloud resources, cloud providers use identity management services such as IAM roles (AWS and GCP) and Active Directory (Azure). 

| Cloud provider | External documentation link | 
|---|---|
| AWS | [AWS Identity and Access Management (IAM)](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) |
| Azure | [Azure Active Directory ((Azure AD))](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) | 
| Google | [Google Cloud Identity and Access Management (IAM)](https://cloud.google.com/iam/docs/overview) | 
| OpenStack | [Keystone](https://docs.openstack.org/keystone/pike/) |

Cloudbreak utilizes cloud provider’s identity management services via Cloudbreak credential. After launching Cloudbreak on your chosen cloud provider, you must create a Cloudbreak credential, which allows Cloudbreak to authenticate with your cloud provider identity management service. Only after you have completed this step, Cloudbreak can create resources on your behalf. 


#### Authentication with AWS

When launching Cloudbreak on AWS, you must select a way for Cloudbreak to authenticate with your AWS account and create resources on your behalf. While key-based authentication uses your AWS access key and secret key, role-based authentication uses IAM roles.

{!docs/common/aws-launch-authentication-role-based-intro.md!}

**Related links**  
[Authentication](aws-pre.md#authentication)  


#### Authentication with Azure

After launching Cloudbreak on Azure, you are required to create a Cloudbreak credential, which allows Cloudbreak to authenticate with your Azure Active Directory. 

You have two options:

* Interactive: The app and service principal creation and role assignment are fully automated, so the only input that you need to provide to Cloudbreak is your Subscription ID and Directory ID. 

* App-based: The app and service principal creation and role assignment are not automated You must create an Azure Active Directory application registration and then provide its parameters to Cloudbreak, in addition to providing your Subscription ID and Directory ID. 

**Related links**  
[Create Cloudbreak credential](azure-launch.md#create-cloudbreak-credential)  


#### Authentication with GCP

After launching Cloudbreak on GCP, you are required to register a service account in Cloudbreak via creating a Cloudbreak credential. Cloudbreak uses this account to authenticate with the GCP identity management service.

**Related links**  
[Service account](gcp-pre.md#service-account)  


#### Authentication with OpenStack 

After launching Cloudbreak on OpenStack, you are required to create a Cloudbreak credential, which allows Cloudbreak to authenticate with keystone. 


**Related links**  
[Create Cloudbreak credential](os-launch.md#create-cloudbreak-credential)  
