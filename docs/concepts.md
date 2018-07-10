## Core Concepts  

Before using Cloudbreak, you should familiarize yourself with the following concepts.     


### Ambari Blueprints

Ambari blueprints are a declarative definition of a cluster. A blueprint allows you to specify stack, component layout, and configurations to materialize a cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard.  

Ambari blueprints are specified in JSON format. After you provide the blueprint to Cloudbreak, the host groups in the JSON are mapped to a set of instances when starting the cluster, and the specified services and components are installed on the corresponding nodes.

Cloudbreak includes a few default blueprints and allows you to upload your own blueprints.

<a href="../images/cb_arch-blue.png" target="_blank" title="click to enlarge"><img src="../images/cb_arch-blue.png" width="500" title="How Cb uses Ambari blueprints"></a> 

[Source]: <> (Source https://docs.google.com/presentation/d/1Br69oOMZIUwQA_qLGslbZW4UxgvSmpSp7FgYuFVwJkE/edit)

**Related links**  
[Using custom blueprints](blueprints.md)  
[Apache documentation](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) (External)  


### Cloudbreak Credential

After launching Cloudbreak, you must create a Cloudbreak credential for each cloud provider on which you would like to provision clusters. Only after you have completed that step you can start creating clusters. 

Cloudbreak credential allows Cloudbreak to authenticate with the cloud provider and create resources on your behalf. The authentication process varies depending on the cloud provider, but is typically done via assigning a specific IAM role to Cloudbreak which allows Cloudbreak to perform certain actions within your cloud provider account. 

<a href="../images/cb_arch-cred.png" target="_blank" title="click to enlarge"><img src="../images/cb_arch-cred.png" width="500" title="How Cb uses Cloudbreak Credential"></a> 

[Source]: <> (Source https://docs.google.com/presentation/d/1Br69oOMZIUwQA_qLGslbZW4UxgvSmpSp7FgYuFVwJkE/edit)

**Related links**  
[Identity management](security.md#identity-management)  


