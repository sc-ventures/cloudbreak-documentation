## Architecture  

**Cloudbreak deployer** installs Cloudbreak components on your AWS VM. Once these components are deployed, you can use **Cloudbreak application** to create, manage, and monitor clusters. 

### Cloudbreak Deployer Architecture

Cloudbreak deployer includes the following components:

| Component | Description |
|---|---|
| **Cloudbreak Application** | Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari. | 
| **Uluwatu** | This is Cloudbreak web UI, which can be used to create, manage, and monitor clusters. |
| **Cloudbreak Shell** | This is Cloudbreak's command line tool, which can be used to create, manage, and monitor clusters. | 
| **Identity** | This is Cloudbreak's OAuth identity server implementation, which utilizes UAA. |
| **Sultans** | This is Cloudbreak's user management system. | 
| **Periscope** | This is Cloudbreak's autoscaling application, which is responsible for automatically increasing or decreasing the capacity of the cluster when your pre-defined conditions are met. |

> These component names are used in Cloudbreak logs, so for troubleshooting purposes it is useful to know what they refer to.

#### System Level Containers

Cloudbreak deployer utilizes **containerization** - also known as container-based virtualization or application containerization - which is an OS-level virtualization method for deploying and running distributed applications. 

Cloudbreak deployer includes the following system-level containers:

* Consul: Cloudbreak service registry  
* Registrator: Automatically registers/unregisters containers with consul 
* Database: Database container for cloudbreak, autoscaling, and UAA  
* Traefik: Proxy container 
 

### Cloudbreak Application Architecture 

The Cloudbreak application is a web application that communicates with the cloud provider account to create cloud resources on your behalf. Once the cloud resources are in place, Cloudbreak uses Apache Ambari to deploy and configure the cluster on cloud VMs. Once your cluster is deployed, you can use Cloudbreak to scale the cluster.

Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari:

* Cloudbreak uses **Apache Ambari** to provision, manage, and monitor HDP clusters. 

    Ambari **blueprints** are a declarative definition of a cluster. With a blueprint, you can specify stack, component layout, and configurations to materialize an HDP cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard. 
    
* Cloudbreak uses **cloud provider APIs** to create cloud resources required for the HDP clusters. 

    You can define these resources (networks, security groups, VMs and storage, and so on) in the create a cluster wizard in the Cloudbreak web UI. Resources are only provisioned once you create the cluster.  
    
The use of blueprints and templates is illustrated in the following image:

<a href="../images/templates-and-blueprints2.png" target="_blank" title="click to enlarge"><img src="../images/templates-and-blueprints2.png" width="650" title="How Cloudbreak uses templates and blueprints"></a> 

#### SaltStack 

Under the hood, Cloudbreak uses SaltStack to manage nodes of the cluster, install packages, change configuration files, and execute recipes. 
By default Salt master is installed on the same node where Ambari server is installed. 

### Core Concepts

#### Ambari Blueprints

**Ambari blueprints** are a declarative definition of a cluster. With a blueprint, you can specify stack, component layout, and configurations to materialize an HDP cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard. 

To learn more about Ambari blueprints, refer to [Apach e documentation](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints).

#### Cloud Infrastructure

**Cloud infrastructure** consists of the virtual networks, virtual machines, storage, and other resources on which your clusters run. When creating a cluster, you define cloud these infrastructure components that will be provisioned:

| Configuration | Description	 | AWS | Azure | GCP | OpenStack |
|---|---|---|---|---|---|
| Networks | **Virtual networks** provide the networking infrastructure (network, subnet, Internet gateway, and so on) in which your clusters run. You can create new virtual networks or reuse existing virtual networks for your clusters. <p>In addition, **security groups** include rules which define inbound traffic allowed to the instances in your cluster.</p> | [Amazon VPC](https://aws.amazon.com/documentation/vpc/) | [Virtual network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) | [Virtual Private Cloud](https://cloud.google.com/compute/docs/vpc/) | Networks |
| VMs and Storage | You can select VM types and their attached storage, including storage type, size, count, and encryption settings. | [Amazon EC2](https://aws.amazon.com/documentation/ec2/) | [Virtual machines](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-linux-azure-overview?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | [Compute Engine](https://cloud.google.com/compute/docs/) | Instances |

#### Cloudbreak Credential

**Cloudbreak credential** allows Cloudbreak to authenticate with the cloud provider and create resources on your behalf. This is typically done via assigning a specific IAM role to Cloudbreak which allows Cloudbreak to perform certain actions.

After launching Cloudbreak, you must create a Cloudbreak credntial and only after you complete that step you can start creating clusters.


