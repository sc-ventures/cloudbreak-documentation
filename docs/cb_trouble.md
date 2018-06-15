# Troubleshooting Cloudbreak

## Getting help

If you need help with Cloudbreak, you have two options:

| Option | Description |
|---|---|
| [Hortonworks Community Connection](#hcc) |	This is free optional support via Hortonworks Community Connection (HCC).|
| [Hortonworks Flex Support Subscription](#flex-subscription) | This is paid Hortonworks enterprise support.|


### HCC

You can register for optional free community support at [Hortonworks Community Connection](https://community.hortonworks.com/answers/index.html) where you can browse articles and previously answered questions, and ask questions of your own. When posting questions related to Cloudbreak, make sure to use the "Cloudbreak" tag.


### Flex subscription

You can optionally use your existing Hortonworks [flex support subscription(s)](https://hortonworks.com/services/support/enterprise/) to cover the Cloudbreak node and clusters managed by it. 

> You must have an existing SmartSense ID and a Flex subscription. For general information about the Hortonworks Flex Support Subscription, visit the Hortonworks Support page at [https://hortonworks.com/services/support/enterprise/](https://hortonworks.com/services/support/enterprise/).

The general steps are:

1. Configure Smart Sense in your `Profile` file.   
2. Register your Flex subscription in the Cloudbreak web UI. You can register and manage multiple Flex subscriptions. For example, you can choose to use your Flex subscription to cover the Cloudbreak node.   
3. When creating a cluster, in the **General Configuration** > **Flex Subscription**, you can select the Flex subscription that you want to use for the cluster.  


#### Configuring SmartSense

To configure SmartSense in Cloudbreak, enable SmartSense and add your SmartSense ID to the `Profile` by adding the following variables:

<pre>export CB_SMARTSENSE_CONFIGURE=true
export CB_SMARTSENSE_ID=YOUR-SMARTSENSE-ID</pre>
    
For example:
 
<pre>export CB_SMARTSENSE_CONFIGURE=true
export CB_SMARTSENSE_ID=A-00000000-C-00000000</pre>

You can do this in one of the two ways:

* When initiating Cloudbreak deployer  
* After you've already initiated Cloudbreak deployer. If you choose this option, you must restart Cloudbreak using `cbd restart`.


#### Register and manage flex subscriptions

Once you log in to the Cloudbreak web UI, you can manage your Flex subscriptions from the **Settings** page > **Flex Subscriptions**:

<a href="../images/cb_cb-flex-settings.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-flex-settings.png" width="650" title="Autoscaling in Cloudbreak UI"></a>  

You can:
 
* Register a new Flex subscription    
* Set a default Flex subscription ("Default")  
* Select a Flex subscription to be used for the Cloudbreak node ("Use for controller")  
* Delete a Flex subscription    

[comment]: <> (This is not implemented yet: Check which clusters are connected to a specific subscription.)  



#### Use flex subscription for a cluster 

When creating a cluster, on the **General Configuration** page you can select the Flex subscription that you want to use for the cluster:

<a href="../images/cb_cb-flex-cluster.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-flex-cluster.png" width="650" title="Autoscaling in Cloudbreak UI"></a>  


#### Use flex subscription for Cloudbreak node

To use a Flex subscription for Cloudbreak node, on the **Settings** page, in the **Flex Subscriptions** section, check the "Use for controller option" for the selected Flex ID.  


### More Cloudbreak resources 

Check out the following documentation to learn more:

<table>
<tr><th width="25%"> Resource </th><th width="75%">Description</th><tr>
<tr><td><a href="http://docs.hortonworks.com/index.html" target="_blank">Hortonworks documentation </a></td>
<td><p>During cluster create process, Cloudbreak automatically installs Ambari and sets up a cluster for you. After this deployment is complete, refer to the <a href="http://docs.hortonworks.com/HDPDocuments/Ambari/Ambari-2.4.1.0/index.html" target="_blank">Ambari documentation</a> and <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/index.html" target="_blank">HDP documentation</a> for help.</p></td>
</tr>
<tr><td>
<a href="http://hortonworks.com/tutorials/" target="_blank">Hortonworks tutorials</a>
</td>
<td>Use Hortonworks tutorials to get started with Apache Spark, Apache Hive, Apache Zeppelin, and more.</td></tr>
<tr><td><a href="https://www.apache.org/" target="_blank">Apache documentation</a></td>
<td>
<p> In addition to Hortonworks documentation, refer to the Apache Software Foundation documentation to get information on specific Hadoop services. 
</p>
</td></tr>
<tr><td><a href="https://cwiki.apache.org/confluence/display/AMBARI/Blueprints" target="_blank">Ambari blueprints</a></td><td>Learn about Ambari blueprints. Ambari blueprints are a declarative definition of a Hadoop cluster that Ambari can use to create Hadoop clusters.</td></tr>
<tr><td><a href="http://hortonworks.com/open-source/cloudbreak/" target="_blank">Cloudbreak project</a></td><td>Visit the Hortonworks website to see Cloudbreak-related news and updates.</td></tr>
<tr><td><a href="http://hortonworks.com/hadoop/ambari/" target="_blank">Apache Ambari project</a></td><td>Learn about the Apache Ambari project. Apache Ambari is an operational platform for provisioning, managing, and monitoring Apache Hadoop clusters. Ambari exposes a robust set of REST APIs and a rich web interface for cluster management.</td></tr>
</table>
 


## Checking Cloudbreak logs

[comment]: <> (TO-DO: How about 'cbd doctor'? I read in the Cloudbreak docs that "The doctor command helps you diagnose problems with your environment, such as common problems with your docker or boot2docker configuration. You can also use it to check cbd versions.") 

When troubleshooting, you can access the following Cloudbreak logs.

### Cloudbreak logs

When installing Cloudbreak using a pre-built cloud image, the  Cloudbreak deployer location and the cbd root folder is `/var/lib/cloudbreak-deployment`. You must execute all cbd actions from the cbd root folder as a cloudbreak user. 

> Your cbd root directory may be different if you installed Cloudbreak on your own VM. 


#### Aggregated logs

Cloudbreak consists of multiple microservices deployed into Docker containers. 

To check aggregated service logs, use the following commands:

`cbd logs` shows all service logs.

`cbd logs | tee cloudbreak.log` allows you to redirect the input into a file for sharing these logs.

#### Individual service logs

To check individual service logs, use the following commands:

`cbd logs cloudbreak` shows Cloudbreak logs. This service is the backend service that handles all deployments.

`cbd logs uluwatu` shows Cloudbreak UI logs. Uluwatu is the UI component of Cloudbreak.

`cbd logs identity` shows Identity logs. Identity is responsible for authentication and authorization.

`cbd logs periscope` shows Periscope logs. Periscope is responsible for triggering autoscaling rules.

#### Docker logs

The same logs can be accessed via Docker commands:

`docker logs cbreak_cloudbreak_1` shows the same logs as `cbd logs cloudbreak`.

Cloudbreak logs are rotated and can be accessed later from the Cloudbreak deployment folder. Each time you restart the application via cbd restart a new log file is created with a timestamp in the name (for example, cbreak-20170821-105900.log). 

> There is a symlink called `cbreak.log` which points to the latest log file. Sharing this symlink does not share the log itself.
 

### Saltstack logs

Cloudbreak uses Saltstack to install Ambari and the necessary packages for the HDP/HDF provisioning. Salt Master always runs alongside the Ambari Server node. Each instance in the cluster runs a Salt Minion, which connects to the Salt Master. There can be multiple Salt Masters if the cluster is configured to run in HA (High Availability) mode and in this case each Salt Minion connects to each Salt Master.

Cloudbreak also uses SaltStack to execute user-provided customization scripts called "recipes". 

Salt Master and Salt Minion logs can be found at the following location: `/var/log/salt`


### Ambari logs

Cloudbreak uses Ambari to orchestrate the installation of the different HDP/HDF components. Each instance in the cluster runs an Ambari agent which connects to the Ambari server. Ambari server is declared by the user during the cluster installation wizard. 

#### Ambari server logs

Ambari server logs can be found on the nodes where Ambari server is installed in the following locations:

`/var/log/ambari-server/ambari-server.log`

`/var/log/ambari-server/ambari-server.out`

Both files contain important information about the root cause of a certain issue so it is advised to check both.

#### Ambari agent logs

Ambari agent logs can be found on the nodes where Ambari agent is installed in the following locations:

`/var/log/ambari-agent/ambari-agent.log`

[comment]: <> (This doc http://hortonworks.github.io/cloudbreak-docs/release-1.16.4/operations/#ambari-server-node mentions more HDP/Ambari logs than these mentioned above. It says: "You can access Hadoop logs from the host and from the container in the /hadoopfs/fs1/logs directory." and "You can access Ambari logs from the host instance in the `/hadoopfs/fs1/logs folder." How are these logs different than these mentioned above?)


### Recipe logs

Cloudbreak supports "recipes" - user-provided customization scripts that can be run prior to or after cluster installation. It is the user’s responsibility to provide an idempotent well tested script. If the execution fails, the recipe logs can be found at `/var/log/recipes` on the nodes on which the recipes were executed.

It is advised, but not required to have an advanced logging mechanism in the script, as Cloudbreak always logs every script that are run. Recipes are often the sources of installation failures as users might try to remove necessary packages or reconfigure services.

## Troubleshooting Cloudbreak

This section includes common errors and steps to resolve them. 


### Invalid PUBLIC_IP in CBD Profile

The `PUBLIC_IP` property must be set in the cbd Profile file or else you won’t be able to log in on the Cloudbreak UI. 

If you are migrating your instance, make sure that after the start the IP remains valid. If you need to edit the `PUBLIC_IP` property in Profile, make sure to restart Cloudbreak using `cbd restart`.


### Cbd cannot get VM's public IP 

By default the `cbd` tool tries to get the VM's public IP to bind Cloudbreak UI to it. But if `cbd` cannot get the IP address during the initialization, you must set it manually. Check your `Profile` and if `PUBLIC_IP` is not set, add the `PUBLIC_IP` variable and set it to the public IP of the VM. For example: 

<pre>export PUBLIC_IP=192.134.23.10</pre>


### Permission or connection problems 

[comment]: <> (Not sure what this refers to. It came from the Install on Your Own VM docs.)

If you face permission or connection issues, disable SELinux:

1. Disable SELINUX:

    <pre>setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config</pre>
 
3. Ensure the SELinux is not turned on afterwards:

    <pre>sestatus | grep -i mode
Current mode:                   permissive
Mode from config file:          permissive</pre>
 

### Creating cbreak_sultans_1 ... Error

`cbd start` returns the following error:

<pre>Creating cbreak_sultans_1 ... error

ERROR: for cbreak_sultans_1  Cannot create container for service sultans: unknown log opt 'max-size' for journald log driver
Creating cbreak_consul_1
Creating cbreak_logrotate_1 ... error
Creating cbreak_periscope_1 ... error
Creating cbreak_mail_1 ... error
Creating cbreak_haveged_1 ... error

ERROR: for cbreak_mail_1  Cannot create container for service mail: unknown log opt 'max-size' for journald log driver

Creating cbreak_uluwatu_1 ... error

Creating cbreak_smartsense_1 ... error

Creating cbreak_consul_1 ... error
Creating cbreak_identity_1 ... error

ERROR: for cbreak_identity_1  Cannot create container for service identity: unknown log opt 'max-file' for journald log driver
Creating cbreak_logsink_1 ... error
Creating cbreak_commondb_1 ... error

ERROR: for cbreak_commondb_1  Cannot create container for service commondb: unknown log opt 'max-size' for journald log driver

ERROR: for haveged  Cannot create container for service haveged: unknown log opt 'max-size' for journald log driver

ERROR: for uluwatu  Cannot create container for service uluwatu: unknown log opt 'max-size' for journald log driver

ERROR: for consul  Cannot create container for service consul: unknown log opt 'max-size' for journald log driver

ERROR: for commondb  Cannot create container for service commondb: unknown log opt 'max-size' for journald log driver

ERROR: for logrotate  Cannot create container for service logrotate: unknown log opt 'max-size' for journald log driver

ERROR: for periscope  Cannot create container for service periscope: unknown log opt 'max-size' for journald log driver

ERROR: for sultans  Cannot create container for service sultans: unknown log opt 'max-size' for journald log driver

ERROR: for mail  Cannot create container for service mail: unknown log opt 'max-size' for journald log driver

ERROR: for logsink  Cannot create container for service logsink: unknown log opt 'max-size' for journald log driver

ERROR: for smartsense  Cannot create container for service smartsense: unknown log opt 'max-size' for journald log driver

ERROR: for identity  Cannot create container for service identity: unknown log opt 'max-file' for journald log driver
Encountered errors while bringing up the project.</pre>

This means that your [Docker logging drivers](https://docs.docker.com/config/containers/logging/configure/) are not configured correctly.

To resolve the issue, on your Cloudbreak VM:

1. Check the Docker Logging Driver configuration:

    <pre>docker info | grep "Logging Driver"</pre>
    
    If it is set to "Logging Driver: journald", you must set it to "json-file". 
    
2. Open the `docker` file for editing:

    <pre>vi /etc/sysconfig/docker</pre>
    
2. Edit the following part of the file so that it looks like below (showing `log-driver=json-file`):

    <pre># Modify these options if you want to change the way the docker daemon runs
OPTIONS='--selinux-enabled --log-driver=json-file --signature-verification=false'</pre>     

3. Restart Docker:

    <pre>systemctl restart docker
systemctl status docker</pre>
        

[Comment]: <> (More: https://drive.google.com/drive/u/0/folders/1Ml8hU3pgphYt47LLWHilRpGQEo6sNee3) 



## Troubleshooting Cloudbreak on AWS

> Check out [HDCloud](http://hortonworks.github.io/hdp-aws/trouble/index.html) troubleshooting docs.

### Unable to create an IAM Role for Cloudbreak 

Most corporate AWS users are unable to create AWS roles. You may have to contact your AWS admin to create the role(s) for you. 

### Cluster fails with a permissions related error

Make sure that the policy attached to CredentialRole includes all actions defined in [CredentialRole](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/cb-policy.json).  


## Troubleshooting Cloudbreak on Azure 

### Cloudbreak deployment errors 

#### Invalid resource reference

Example error message:  
*<span class="cfn-output3">Resource /subscriptions/.../resourceGroups//providers/Microsoft.Network/virtualNetworks/cbdeployerVnet/  
subnets/cbdeployerSubnet referenced by resource /subscriptions/.../resourceGroups/Manulife-ADLS/providers/  
Microsoft.Network/networkInterfaces/cbdeployerNic was not found.  
Please make sure that the referenced resource exists, and that both resources are in the same region.</span>*

**Symptom**: The most common reason for this error is that you did not provide the Vnet RG Name (last parameter in the template).  

**Solution**: When launching Cloudbreak, under "Vnet RG Name" provide the name of the resource group in which the selected VNet is located. If using a new VNet, enter the same resource group name as in "Resource group". 

### Credential prerequisite errors

#### You don't have enough permissions to assign roles 

This error during the interactive credential creation typically means that you do not have suitable permissions to create an interactive credential. Using an interactive credential currently requires an "Owner" role or its equivalent so if you are using a corporate account you are unlikely to have it. Try using the app-based credential. 

#### Problems with IAM permissions assignment 

After registering an Azure application you may have to ask your Azure administrator to perform the step of assigning the "Contributor" role to it:

<img src="../images/cb_azure-appbased03.png" width="650" title="Azure Portal"> 


### Credential creation errors

#### Role already exists

Example error message: *<span class="cfn-output3">Role already exists in Azure with the name: CloudbreakCustom50</span>*

**Symptom**: You specified that you want to create a new role for Cloudbreak credential, but an existing role with the same name already exists in Azure. 

**Solution**: You should either rename the role during credential creation or select the `Reuse existing custom role` option. 

#### Role does not exist

Example error message: *<span class="cfn-output3">Role does not exist in Azure with the name: CloudbreakCustom60</span>*

**Symptom**: You specified that you want to reuse an existing role for your Cloudbreak credential, but that particular role does not exist in Azure.

**Solution**: You should either rename the new role during the credential creation to match the existing role's name or select the `Let Cloudbreak create a custom role` option. 

#### Role does not have enough privileges 

Example error message: *<span class="cfn-output3">CloudbreakCustom 50 role does not have enough privileges to be used by Cloudbreak!</span>  
<span class="cfn-output3"></span>*

**Symptom**: You specified that you want to reuse an  existing role for your Cloudbreak credential, but that particular role does not have the necessary privileges for Cloudbreak cluster management.

**Solution**: You should either select an existing role with enough privileges or select the `Let Cloudbreak create a custom role` option.
 
The necessary action set for Cloudbreak to be able to manage the clusters includes:
        `"Microsoft.Compute/*",
        "Microsoft.Network/*",
        "Microsoft.Storage/*",
        "Microsoft.Resources/*"`
 
#### Client does not have authorization  

Example error message:  
*<span class="cfn-output3">Failed to verify credential: Status code 403, {"error":{"code":"AuthorizationFailed",  
"message":"The client 'X' with object id 'z' does not have authorization to perform action  
'Microsoft.Storage/storageAccounts/read' over scope 'subscriptions/...'"}</span>*

**Symptom**: Your Azure account does not have sufficient permissions to create a Coudbreak credential. 

**Solution**: If you get this error during interactive credential creation, please ensure that your Azure account has `Microsoft.Authorization/*/Write` permission. Otherwise contact your Azure administrator to either give your account that permission or create the necessary resources for the app-based credential creation method.  
 
#### Cloud not validate publickey certificate

Example error message:  
*<span class="cfn-output3">Could not validate publickey certificate [certificate: 'fdfdsf'], detailed message:   
Corrupt or unknown public key file format</span>*

**Symptom**: The syntax of your SSH public key is incorrect.

**Solution**: You must correct the syntax of your SSH key. For information about the correct syntax, refer to [this](https://tools.ietf.org/html/rfc4716#section-3.6) page.


## Troubleshooting Cloudbreak on GCP

### Google Cloud create cluster fails with permissions related error

In order to launch clusters on GCP via Cloudbreak, you must have a Service Account that Cloudbreak can use to create resources. In addition, you must also have a P12 key associated with that account.

Usually, a user with an "Owner" role can assign roles to new and existing service accounts from **IAM & Admin > IAM** in the Google Cloud console. If you are using your own account, you should be able to perform this step, but if you are using a corporate account, you will likely have to contact your Google Cloud admin.

The roles for the service account are described in [Service account](https://hortonworks.github.io/cloudbreak-documentation/latest/). 


## Troubleshooting cluster creation

### Configure communication via private IPs on AWS

Cloudbreak uses public IP addresses when communicating with cluster nodes. On AWS, you can configure it to use private IPs instead of public IPs by setting the CB_AWS_VPC variable in the Profile. 

> This configuration is available for AWS only. Do not use it for other cloud providers. 

1. Navigate to the Cloudbreak deployment directory and edit Profile. For example:

    <pre>cd /var/lib/cloudbreak-deployment/
vi Profile</pre>

2. Add the following entry, setting it to the AWS VPC identifier where you have deployed Cloudbreak:

    <pre>export CB_AWS_VPC=your-VPC-ID</pre>

    For example:
    
    <pre>export CB_AWS_VPC=vpc-e261a185</pre>
    
3. Restart Cloudbreak by using `cbd restart`.      

 
### Cannot access Oozie web UI

Ext JS is GPL licensed software and is no longer included in builds of HDP 2.6. Because of this, the Oozie WAR file is not built to include the Ext JS-based user interface unless Ext JS is manually installed on the Oozie server. If you add Oozie using Ambari 2.6.1.0 to an HDP 2.6.4 or greater stack, no Oozie UI will be available by default. Therefore, if you plan to use Oozie web UI with Ambari 2.6.1.0 and HDP 2.6.4 or greater, you you must manually install Ext JS on the Oozie server host.

You can install Ext JS by adding the following PRE-AMBARI-START recipe:

<pre>export EXT_JS_VERSION=2.2-1
     export OS_NAME=centos6
     wget http://public-repo-1.hortonworks.com/HDP-UTILS-GPL-1.1.0.22/repos/$OS_NAME/extjs/extjs-$EXT_JS_VERSION.noarch.rpm
     rpm -ivh extjs-$EXT_JS_VERSION.noarch.rpm</pre> 
     
Make the following changes to the script:

* Change the EXT_JS_VERSION to the specific ExtJS version that you want to use.  
* Change the OS_NAME to the name of the operating system. Supported values are: centos6, centos7, centos7-ppc.

The general steps are:

1. Be sure to review and agree to the Ext JS license prior to using this recipe.  
2. Create a PRE-AMBARI-START recipe. For instructions on how to create a recipe, refer to [Add recipes](https://hortonworks.github.io/cloudbreak-documentation/latest/).   
3. When creating a cluster, choose this recipe to be executed on all host groups of the cluster. 

 


### Quota limitations

Each cloud provider has quota limitations on various cloud resources, and these quotas can usually be increased on request. If there is an error message in Cloudbreak stating that there are no more available EIPs (Elastic IP Address) or VPCs, you need to request more of these resources. 

To see the limitations visit the cloud provider’s site:

* [AWS service limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) 
* [Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits)
* [GCP resource quotas](https://cloud.google.com/compute/quotas) 

### Connection timeout when ports are not open

In the cluster installation wizard, you must specify on which node you want to run the Ambari server. Cloudbreak communicates with this node to orchestrate the installation.

A common reason for connection timeout is security group misconfiguration. Cloudbreak allows configuring different security groups for the different instance groups; however, there are certain requirements for the Ambari server node. Specifically, the following ports must be open in order to communicate with that node:

* 22 (SSH)  
* 9443 (two-way-ssl through nginx) 


### Blueprint errors 

#### Invalid services and configurations

Ambari blueprints are a declarative definition of a cluster. With a blueprint, you specify a stack, the component layout, and the configurations to materialize a Hadoop cluster instance via a REST API without having to use the Ambari cluster install wizard. 

Cloudbreak supports any type of blueprints, which is a common source of errors. These errors are only visible once the core infrastructure is up and running and Cloudbreak tries to initiate the cluster installation through Ambari. Ambari validates the blueprint and  rejects it if it's invalid. 

For example, if there are configurations for a certain service like Hive but Hive as a service is not mapped to any host group, the blueprint is invalid.

To fix these type of issues, edit your blueprint and then reinstall your cluster. Cloudbreak UI has support for this so the infrastructure does not have to be terminated.

There are some cases when Ambari cannot validate your blueprint beforehand. In these cases, the issues are only visible in the Ambari server logs. To troubleshoot, check Ambari server logs.


#### Wrong HDP/HDF version

In the blueprint, only the major and minor HDP/HDF version should be defined (for example, "2.6"). If wrong version number is provided, the following error can be found in the logs:

```
5/15/2017 12:23:19 PM testcluster26 - create failed: Cannot use the specified Ambari stack: HDPRepo
{stack='null'; utils='null'}
. Error: org.apache.ambari.server.controller.spi.NoSuchResourceException: The specified resource doesn't exist: Stack data, Stack HDP 2.6.0.3 is not found in Ambari metainfo
```

For correct blueprint layout, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page.
  

### Recipe errors 

#### Recipe execution times out

If the scripts are taking too much time to execute, the processes will time out, as the threshold for all recipes is set to 15 minutes. To change this threshold, you must override the default value by adding the following to the cbd Profile file:


```
export CB_JAVA_OPTS=” -Dcb.max.salt.recipe.execution.retry=90”
``` 

This property indicates the number of tries for checking if the scripts have finished with a sleep time (i.e. the wait time between two polling attempts) of 10 seconds. The default value is 90. To increase the threshold, provide a number greater than 90. You must restart Cloudbreak after changing properties in the Profile file.


#### Recipe execution fails

It often happens that a script cannot be executed successfully because there are typos or errors in the script. To verify this you can check the recipe logs at
`/var/log/recipes`. For each script, there will be a separate log file with the name of the script that you provided on the Cloudbreak UI.


## Troubleshooting Cloudbreak CLI

### Special characters in blueprint name cause an error  

When registering a blueprint via `blueprint create` CLI command, if the name of the blueprint includes one or more of the following special characters `@#$%|:&*;` you will get an error similar to:  

<pre>cb blueprint create from-url --name test@# --url https://myurl.com/myblueprint.bp  
[1] 7547
-bash: application.yml: command not found
-bash: --url: command not found
 ~  integration-test  1  time="2018-02-01T12:56:44+01:00" level="error" msg="the following parameters are missing: url\n"</pre> 
 
*Solution:*  
When using special characters in a blueprint name, make sure to use quotes; for example "test@#":  

<pre>cb blueprint create from-url --name "test@#" --url https://myurl.com/myblueprint.bp</pre>
 






