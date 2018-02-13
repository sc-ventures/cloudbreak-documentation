## Release Notes

### 2.4.0

Cloudbreak 2.4.0 is a general availability release, which is suitable for production deployments. 

____________________________

#### New Features
____________________________

##### New UI/UX

Cloudbreak 2 introduces a new user interface, designed to make user interaction with Cloudbreak easier.

##### New CLI

Cloudbreak 2.1.0 TP introduces a new CLI tool. All commands start with a singular object followed by an action, for example, blueprint create instead of create blueprint, and blueprint list instead of list blueprints. Refer to [Install CLI](cli-install.md) and [CLI Reference](cli-reference.md).

##### Support for Kerberos

Creating Kerberos-enabled clusters is supported. Refer to [Enabling Kerberos Security](security-kerberos.md).

##### Support for Configuring an External RDBMS for Cloudbreak

Using an external RDBMS for Cloubdreak is supported. For configuration instructions, refer to [Configuring External Cloudbreak Database](cb-db.md).

##### Support for Migrating Cloudbreak Instance

Migrating Cloudbreak from one machine to another is supported. For migration instructions, refer to [Moving a Cloudbreak Instance](cb-migrate.md).

##### Providing Your Own JDK

Providing your own JDK on a custom base image is supported. For instructions, refer to "Advanced topics" in the [https://github.com/hortonworks/cloudbreak-images](https://github.com/hortonworks/cloudbreak-images) repository.

##### Prewarmed Images 

To accelerate cluster creation, CLoudbreak 2.4 introduces prewarmed images, which include the operating system, as well as the default version of Ambari and HDP. By default, Cloudbreak 2.4 launches clusters from these prewarmed images, instead of using base images (which were used by default in earlier versions of Cloudbreak). Default base images are still available in case you would like to use different Ambari and HDP versions than those provided with prewarmed images. For more information, refer to [Prewardmed and Base Images](aws-create.md#prewarmed-and-base-images). 

##### CLI Templates 

After specifying the parameters for your cluster in the Cloudbreak web UI, you can copy the content of the CLI JSON file that can be used to create a cluster via Cloudbreeak CLI. For more information, refer to [Obtain Cluster JSON Template from the UI](cli-get-started.md#obtain-cluster-json-template-from-the-ui).    

Furthermore, Cloudbreak web UI includes an option in the UI which allows you to generate the  `create` command for resources such as credentials, blueprints, clusters, and recipes. For more information, refer to [Obtain CLI Command from the UI](cli-get-started.md#obtain-cli-command-from-the-ui).  

##### New Recipe Types 

New types of recipes are introduced:

* PRE-AMBARI-START (new, useful for configuring Ambari prior to start)  
* POST-AMBARI-START (formerly known as PRE)  
* POST-CLUSTER-INSTALL (formerly known as POST)  
* PRE-TERMINATION (new, useful for cluster cleanup pre-termination tasks)  
Refer to updated [Recipes](recipes.md) documentation.

##### Disabling Cloud Providers 

You can hide cloud providers available in Cloudbreak by adding the `CB_ENABLEDPLATFORMS` environment variable in Profile and setting it to the provider(s) that you would like to have available. For more information, refer to [Disable Providers](cb-disable-provider.md).

##### Support for Ambari 2.6 (2.6.1.3+)

Cloudbreak 2.4.0 introduces support for Ambari 2.6 and uses Ambari 2.6.1.3 by default. If you would like to use Ambari 2.6, you must use Ambari version 2.6.1.3 or newer. 

##### Ambari Master Key 

[Comment]: <> (Not sure if this is a new feature or if it was available in Cloudbreak 1.16?)





____________________________

#### Behavioral Changes
____________________________

##### Custom Images

The functionality which enables you to create custom images was changed and improved. Refer to [Custom Images](images.md).

##### Prewarmed Images Are Used By Default  

By default, Cloudbreak 2.4 launches clusters from these prewarmed images, instead of base images (which were used by default in earlier versions of Cloudbreak). For more information, refer to [Prewardmed and Base Images](aws-create.md#prewarmed-and-base-images). 

##### Removal of Cloudbreak Shell

Cloudbreak Shell is no longer available in Cloudbreak 2.1.0 TP and later. It was replaced by the [Cloudbreak CLI](cli-install.md).

##### Removal of Platforms

The [Platforms](http://hortonworks.github.io/cloudbreak-docs/release-1.16.5/topologies/) feature was removed.

##### Removal of Mesos

Cloudbreak 2 does not support Mesos cloud provider.

##### Removal of Templates

Earlier versions of Cloudbreak allowed you to save infrastructure, network, and security group templates. This feature was removed. Instead, you can define VMs, storage, networks, and security groups as part of the create cluster wizard.

[Comment]: <> (Add GPL changes?)





____________________________

#### Fixed Issues 
____________________________


[Comment]: <> (Not sure which issues were fixed when comparing 2.x with 1.6.x?)



____________________________

#### Known Issues
____________________________








##### (BUG-92605) Cluster Creation Fails with ResourceInError

Cluster creation fails with the following error: 

*Infrastructure creation failed. Reason: Failed to create the stack for CloudContext{id=3689, name='test-exisitngnetwork', platform='StringType{value='OPENSTACK'}', owner='e0307f96-bd7d-4641-8c8f-b95f2667d9c6'} due to: Resource CREATE failed: ResourceInError: resources.ambari_volume_master_0_0: Went to status error due to "Unknown"*

*Workaround*: 

This may mean that the volumes that you requested exceed volumes available on your cloud provider account. When creating a cluster, on the advanced *Hardware and Storage* page of the create cluster wizard, try reducing the amount of requested storage. If you need more storage, try using a different region or ask your cloud provider admin to increase the resource quota for volumes.  

[Comment]: <> (This jira item was closed so it will not be fixed. Maybe add this to troubleshooting?)
____________________________








##### (BUG-91013) Incorrect Node Status After Cluster Restart 

You may sporadically experience an issue where after you stop and restart a cluster, the node status displayed in the "Hardware" section is incorrect.   

[comment]: <> (Not sure what the workaround is for BUG-91013?)
____________________________





##### (BUG-93241) Error When Scaling Multiple Host Groups 

Scaling of multiple host groups fails with the following error: 
*Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1; nested exception is org.hibernate.StaleStateException: Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1*

*Workaround:*

Scaling multiple host groups at once is not supported. If you would like to scale multiple host groups: scale the first host group and wait until scaling has completed, then scale the second host group, and so on.  
____________________________





##### (AMBARI-14149) Ambari Cluster Could Not Be Started After Stop

When using Ambari version 2.5.0.3, after stopping and starting a cluster, Event History shows the following error:

*Ambari cluster could not be started. Reason: Failed to start Hadoop services.
2/7/2018, 12:47:05 PM
Starting Ambari services.
2/7/2018, 12:47:04 PM
Manual recovery is needed for the following failed nodes:   
[host-10-0-0-4.openstacklocal, host-10-0-0-3.openstacklocal, host-10-0-0-5.openstacklocal*

Ambari dashboard shows that nodes are not sending heartbeats. 

 *Workaround:*  
 
 This issue is fixed in 2.5.1.0 and newer.  

[Comment]: <> (See BUG-96086, EAR-6780, AMBARI-14149)
____________________________



##### (BUG-95607) Special Characters in Blueprint Name Cause an Error in CLI  

When registering a blueprint via `blueprint create` CLI command, if the name of the blueprint includes one or more of the following special characters `@#$%|:&*;` you will get an error similar to:  

*cb blueprint create from-url --name test@# --url https://myurl.com/myblueprint.bp  
[1] 7547
-bash: application.yml: command not found
-bash: --url: command not found
 ~  integration-test  1  time="2018-02-01T12:56:44+01:00" level="error" msg="the following parameters are missing: url\n"* 
 
 *Workaround:*  
 You have two options:
 
* Do not include any of the following special characters `@#$%|:&*;` in the blueprint name.  
* If you want to use special characters in the name, perform the task via the UI.  
____________________________






##### (BUG-93257) Clusters Are Missing From History   

After changing the dates on the *History* page multiple times, the results displayed may sometimes be incorrect. 

*Workaround:*

Refresh the page if you think that the history displayed may be incorrect.  
____________________________





