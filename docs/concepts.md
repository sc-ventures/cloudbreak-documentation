## Core concepts  

Before using Cloudbreak, you should familiarize yourself with the following concepts.     


### Ambari blueprints

Ambari blueprints are a declarative definition of a cluster. A blueprint allows you to specify stack, component layout, and configurations to materialize a cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard.  

Ambari blueprints are specified in JSON format. After you provide the blueprint to Cloudbreak, the host groups in the JSON are mapped to a set of instances when starting the cluster, and the specified services and components are installed on the corresponding nodes.

Cloudbreak includes a few default blueprints and allows you to upload your own blueprints.

<a href="../images/cb_arch-blue.png" target="_blank" title="click to enlarge"><img src="../images/cb_arch-blue.png" width="500" title="How Cb uses Ambari blueprints"></a> 

[Source]: <> (Source https://docs.google.com/presentation/d/1Br69oOMZIUwQA_qLGslbZW4UxgvSmpSp7FgYuFVwJkE/edit)

**Related links**  
[Using custom blueprints](blueprints.md)  
[Apache documentation](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) (External)  


### Cloudbreak credential

After launching Cloudbreak, you must create a Cloudbreak credential for each cloud provider on which you would like to provision clusters. Only after you have completed that step you can start creating clusters. 

Cloudbreak credential allows Cloudbreak to authenticate with the cloud provider and create resources on your behalf. The authentication process varies depending on the cloud provider, but is typically done via assigning a specific IAM role to Cloudbreak which allows Cloudbreak to perform certain actions within your cloud provider account. 

<a href="../images/cb_arch-cred.png" target="_blank" title="click to enlarge"><img src="../images/cb_arch-cred.png" width="500" title="How Cb uses Cloudbreak Credential"></a> 

[Source]: <> (Source https://docs.google.com/presentation/d/1Br69oOMZIUwQA_qLGslbZW4UxgvSmpSp7FgYuFVwJkE/edit)

**Related links**  
[Identity management](security.md#identity-management)  


### Data lake

A data lake provides a way for you to centrally apply and enforce authentication, authorization, and audit policies across multiple ephemeral workload clusters. "Attaching" your workload cluster to the data lake instance allows the attached cluster workloads to access data and run in the security context provided by the data lake. 

**Related links**  
[Setting up a data lake](data-lake.md)


### Dynamic blueprints

Production cluster configurations typically include certain configuration parameters, such as those related to external database (for Hive, Ranger, etc) and LDAP/AD, forcing users to create 1+ versions of the same blueprint to handle different component configurations for these external systems.     

Dynamic blueprints offer the ability to manage external sources (such as RDBMS and LDAP/AD) outside of your blueprint, because they merely use the blueprint as a template and Cloudbreak injects the actual configurations into your blueprint. This simplifies the reuse of cluster configurations for external sources (RDBMS and LDAP/AD) and simplifies the blueprints themselves.  

Cloudbreak allows you to create special "dynamic" blueprints which include templating: the values of the variables specified in the blueprint are dynamically replaced in the cluster creation phase, picking up the parameter values that you provided in the Cloudbreak UI or CLI.
Cloudbreak supports [mustache](https://mustache.github.io/) kind of templating with {{{variable}}} syntax.

**Related links**  
[Creating dynamic blueprints](blueprints.md#creating-dynamic-blueprints)

 
### External sources
 
Cloudbreak allows you to define external sources that are created independently of a cluster -- and therefore their lifespan is not limited by the lifespan of any cluster -- and that can be reused with multiple clusters:

<a href="../images/cb_external-source.png" target="_blank" title="click to enlarge"><img src="../images/cb_external-source.png" width="650" title="How Cb uses Cloudbreak Credential"></a> 

[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)

The external sources that can be registered in Cloudbreak include: 

* Authentication configurations (LDAP/AD) 
* Database configurations   
* Image catalogs  
* Proxy configurations   

Once you register an external source, you may reuse it for multiple clusters. 

**Related links**  
[Using an external database for cluster services](external-db.md)   
[Using an external authentication source for clusters](external-ldap.md)   
[Register a proxy](external-proxy.md)  
[Using custom images](images.md)      


### Recipes 

Cloudbreak allows you to upload custom scripts, called "recipes". A recipe is a script that runs on all nodes of a selected node group at a specific time. You can use recipes for tasks such as installing additional software or performing advanced cluster configuration. For example, you can use a recipe to put a JAR file on the Hadoop classpath.

Available recipe execution times are:  

* Before Ambari server start    
* After Ambari server start    
* After cluster installation    
* Before cluster termination   

You can upload your recipes to Cloudbreak via the UI or CLI. Then, when creating a cluster, you can optionally attach one or more "recipes" and they will be executed on a specific host group at a specified time. 

**Related links**  
[Using custom scripts (recipes)](recipes.md) 
