## Architecture  

**Cloudbreak deployer** installs Cloudbreak components on a VM. Once these components are deployed, you can use **Cloudbreak application** or Cloudbreak CLI to create, manage, and monitor clusters. 

### Cloudbreak deployer architecture

**Cloudbreak deployer** installs Cloudbreak components on a VM. It includes the following components:

| Component | Description |
|---|---|
| **Cloudbreak Application** | Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari. | 
| **Uluwatu** | This is Cloudbreak web UI, which can be used to create, manage, and monitor clusters. |
| **Cloudbreak CLI** | This is Cloudbreak's command line tool, which can be used to create, manage, and monitor clusters. | 
| **Identity** | This is Cloudbreak's OAuth identity server implementation, which utilizes UAA. |
| **Sultans** | This is Cloudbreak's user management system. | 
| **Periscope** | This is Cloudbreak's autoscaling application, which is responsible for automatically increasing or decreasing the capacity of the cluster when your pre-defined conditions are met. |
 

### Cloudbreak application architecture 

The Cloudbreak application is a web application which simplifies cluster provisioning in the cloud. Based on your input, Cloudbreak provisions all required cloud infrastructure and then provisions a cluster on your behalf within your cloud provider account.   

<a href="../images/cb_arch.png" target="_blank" title="click to enlarge"><img src="../images/cb_arch.png" width="550" title="Cloudbreak architecture"></a> 

Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari:
    
* Cloudbreak uses **cloud provider APIs** to communicate with the cloud providers. 

* Cloudbreak uses the **Cloudbreak credential** to authenticate with your cloud provider account and provision cloud resources required for the clusters. 
    
* Cloudbreak uses Apache Ambari and **Ambari blueprints** to provision, manage, and monitor clusters. Ambari blueprints are a declarative definition of a cluster. With a blueprint, you can specify stack, component layout, and configurations to materialize a cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard.     
  
  


   
