## Release Notes

### 2.2.0 TP

The Cloudbreak 2.2.0 TP release is technical preview: it is not suitable for production environments.

____________________________

#### New Features
____________________________

##### **Support for Kerberos**
Creating Kerberos-enabled clusters is supported. Refer to [Enabling Kerberos Security](security-kerberos.md). 

##### **Configuring an external RDBMS for Cloudbreak**
Using an external RDBMS for Cloubdreak is supported. For configuration instructions, refer to [Configuring External Cloudbreak Database](cb-db.md).

##### **Migrating Cloudbreak Instance**
Migrating Cloudbreak from one machine to another is supported. For migration instructions, refer to [Moving a Cloudbreak Instance](cb-migrate.md).

##### **Providing Your Own JDK**
Providing your own JDK on a custom base image is supported. For instructions, refer to "Advanced topics" in the [https://github.com/hortonworks/cloudbreak-images](https://github.com/hortonworks/cloudbreak-images) repo.  

#####**Spot Instances on AWS**  
Using spot instances is supported for clusters created on AWS. The option is available in the advanced *Hardware and Storage* section of the create cluster wizard. Refer to [Use Spot Instances](aws-create.md#use-spot-instances).

#####**Preemptible Instances on Google Cloud**  
Using preemptible instances is supported for clusters created on Google Cloud. The option is available in the advanced *Hardware and Storage* section of the create cluster wizard. Refer to [Use Preemptible Instances](aws-create.md#use-preemptible-instances).

#####**New Types of Recipes**
New types of recipes are introduced:

* PRE-AMBARI-START (new, useful for configuring Ambari prior to start)  
* POST-AMBARI-START (formerly known as PRE)
* POST-CLUSTER-INSTALL (formerly known as POST)  

Refer to updated [Recipes](recipes.md) documentation.

#####**Disabling Cloud Providers**
You can hide cloud providers available in Cloudbreak by adding the CB_ENABLEDPLATFORMS environment variable in Profile and setting it to the provider(s) that you would like to have available. For more information, refer to [Disable Providers](cb-disable-provider.md).


#####**New 'cloud' CLI Commands**
New `cloud` commands are available in the CLI, allowing you to get cloud provider details required for a cluster template. Refer to [CLI Reference](cli-reference.md).



____________________________

#### Behavioral Changes
____________________________

#####**Image Catalog Registration**
Image catalog registration is now possible via Cloudbreak web UI. Refer to updated [Register Custom Images](images.md#register-custom-images) documentation.

#####**Create Cluster User Interface Changes**
Some sections in the create cluster wizard were changed to ensure better user experience. For example, the security group section was improved to help you provide desired security groups settings.

#####**Cluster Details User Interface Changes**
The cluster details page was changed to ensure better user experience. New tabs were created to help you find information related to your cluster.

#####**CLI Syntax**
The syntax of all CLI commands has changed. All commands start with a singular object followed by an action, for example, `blueprint create` instead of `create blueprint`, and `blueprint list` instead of `list blueprints`. Refer to updated [CLI Reference](cli-reference.md).


____________________________

#### Fixed Issues
____________________________

The following issues have been fixed in this release:

| Jira |  Description |
|---|---|
| BUG-91768 | The *Add* button used for adding tags does not work. |
| BUG-90848 | "Do Not Use Security Group" option does not work for a new network. |
| BUG-91827 | After cluster has been stopped, Event History shows "Infrastructure Has Been Terminated". |
| BUG-91701 | Cluster resize is unclear: the cluster name and cluster information in the text above the scaling controls are incorrect. |
| BUG-91892 | "Recipe name is already taken" error when using a recipe description longer than 255 characters. |
| BUG-91077 | When usign EDW-ETL: Apache Hive 1.2.1, Apache Spark 1.6 blueprint, nodes are unhealthy after sync due to Druid service. |
| BUG-91835 | Default Ambari node security group Has Port 22, 443, and 9443 inbound CIDR set to 0.0.0.0/0. |



____________________________

#### Known Issues
____________________________

##### (RMP-10114) Auto-scaling Is Not Available

Cluster auto-scaling functionality is not available in Cloudbreak 2.2.0 TP.
____________________________



##### (RMP-10409) Azure Disk Encryption Is Not Available

Azure disk encryption functionality is not available in Cloudbreak 2.2.0 TP.
____________________________



##### (BUG-93556) Hadoop YARN Provider Option in the Create Credential Dropdown  

When creating a Cloudbreak credential, the UI shows "Hadoop YARN" as a cloud provider option.

*Workaround*:

Ignore this option.
____________________________



##### (BUG-93548) AWS Region eu-west-3 Is Not Supported

The AWS region eu-west-3 can be selected during cluster creation. However, it is not supported by Cloudbreak.

*Workaround*:

Do not use the AWS region eu-west-3. Instead, use eu-west-1 or eu-west-2.
____________________________



##### (BUG-91543) Networks With No Subnets Are Not Supported

You cannot create a cluster using an existing network that does not have any subnets. You must use a network that includes at least one subnet. If you try to use a network with no subnets, the cluster fails with the following error:   

*Infrastructure creation failed. Reason: Invalid value for field 'resource.network': 'https://www.googleapis.com/compute/v1/projects/siq-haas/global/networks/cbd-test'. A subnet mode Network must be specified for Subnetwork creation.: [ resourceType: GCP_SUBNET, resourceName: testgc-20171110211021 ]*

*Workaround*:

Do not use this option. It will be removed in a future release.  
____________________________



##### (BUG-93602) Do Not Create Public IPs Azure Cluster Option

If you check "Do not create Public IPs” when creating a cluster on Azure, public IPs are created. This will be fixed in a future release.  
____________________________



##### (BUG-92605) Cluster Creation Fails with ResourceInError

Cluster creation fails with the following error:

*Infrastructure creation failed. Reason: Failed to create the stack for CloudContext{id=3689, name='test-exisitngnetwork', platform='StringType{value='OPENSTACK'}', owner='e0307f96-bd7d-4641-8c8f-b95f2667d9c6'} due to: Resource CREATE failed: ResourceInError: resources.ambari_volume_master_0_0: Went to status error due to "Unknown"*

*Workaround*:

This may mean that the volumes that you requested exceed volumes available on your cloud provider account. When creating a cluster, on the advanced *Hardware and Storage* page of the create cluster wizard, try reducing the amount of requested storage. If you need more storage, try using a different region or ask your cloud provider admin to increase the resource quota for volumes.  
____________________________



##### (BUG-90985) OpenStack Cluster Creation Fails at Network Configuration

When creating a cluster on OpenStack, if you do not select an existing network and subnet, the cluster fails with an error similar to:  
*Infrastructure creation failed. Reason: At least one of the following properties must be specified: network, network_id.*   
*Infrastructure creation failed. Reason: The Parameter (router_id) was not provided.*

*Workaround*:

When creating a cluster on OpenStack, you must select an existing network and subnet.
____________________________



##### (BUG-93339) HiveServer2 Process Could Not Establish Connection to jdbc:hive2  

When using EDW-Analytics: Apache Hive 2 LLAP, Apache Zeppelin 0.7.0 blueprint with a two-node cluster, Ambari shows the following Alert on Hive: *HiveServer2 Process CRIT Connection failed on host...*

*Workaround:*

* Although it is possible to create a cluster with less than 3 nodes, in order to use the EDW-Analytics: Apache Hive 2 LLAP, Apache Zeppelin 0.7.0 blueprint, you must have at least 3 nodes: 1 Ambari node and 2 non-Ambari nodes. Terminate the cluster and create a new one with at least 3 nodes.   
* Alternatively, it is possible to resolve this issue by adding an additional NodeManager for the Ambari host group.  
____________________________



##### (BUG-92581) Unable to Open Ambari in Firefox 57.0 (64-bit)

When trying to access Ambari in the Firefox 57.0 (64-bit) browser, you may get "Secure Connection Failed" with an error code "SEC_ERROR_INADEQUATE_KEY_USAGE".

*Workaround:*

Try using a different browser.
____________________________



##### (BUG-91810) Network and Subnet Are Listed as "N/A" or "New"

When creating a new network and subnet for your cluster, the network and subnet information is unavailable on the cluster details page, showing "N/A" or "New".

*Workaround:*

If you want to check which network and subnet are used for your cluster, navigate to the cloud provider account and find the cluster instances that were created for your cluster. Next, check which virtual network and subnet they are associated with. The steps vary depending on the provider.  
____________________________



##### (BUG-91013) Incorrect Node Status After Cluster Restart

You may sporadically experience an issue where after you stop and restart a cluster, the node status displayed in the "Hardware" section is incorrect.   

[comment]: <> (Not sure what the workaround is for BUG-91013?)
____________________________



##### (BUG-91071) Syntax Error During Cluster Downscale  

When trying to downscale your cluster below the minimum required number of nodes, you may get the following error: SyntaxError: Unexpected end of JSON input.

*Workaround:*

Check the Event History for more information:

* If you tried to scale the cluster below the minimum required number of nodes, you will see: New node(s) could not be removed from the cluster. Reason There is not enough node to downscale. Check the replication factor and the ApplicationMaster occupation. It may take a few minutes for this message to appear in the the Event History.  
* In other cases, you should see a message informing you that downscale was successful. It may take a few minutes for this message to appear in the the Event History.  
____________________________



##### (BUG-93241) Error When Scaling Multiple Host Groups

Scaling of multiple host groups fails with the following error:
*Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1; nested exception is org.hibernate.StaleStateException: Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1*

*Workaround:*

Scaling multiple host groups at once is not supported. If you would like to scale multiple host groups: scale the first host group and wait until scaling has completed, then scale the second host group, and so on.  
____________________________



##### (BUG-93243) Incorrect Recipe Type Is Listed

After adding a post-ambari-start recipe, you may see it incorrectly listed as a post-ambari-install recipe.
____________________________



##### (BUG-93257) Clusters Are Missing From History   

After changing the dates on the *History* page multiple times, the results displayed may sometimes be incorrect.

*Workaround:*

Refresh the page if you think that the history displayed may be incorrect.  
____________________________



##### (RMP-10340) Unable to Dekerberize the Cluster During Cluster Termination  

Cluster with Cloudbreak provisioned Kerberos fails during dekerberizing. The Ambari shows the following error:

*ERROR [ambari-heartbeat-processor-0] ServiceComponentHostImpl:1025 - Can't handle ServiceComponentHostEvent event at current state, serviceComponentName=ZOOKEEPER_CLIENT, hostName=host-10-0-0-3.openstacklocal, currentState=INSTALLED, eventType=HOST_SVCCOMP_STOPPED, event=EventType: HOST_SVCCOMP_STOPPED  
WARN [ambari-heartbeat-processor-0] HeartbeatProcessor:567 - State machine exception. Invalid event: HOST_SVCCOMP_STOPPED at INSTALLED  
ERROR [Server Action Executor Worker 62] PrepareDisableKerberosServerAction:151 - The data directory has not been set.  Generated data can not be stored.  
WARN [Server Action Executor Worker 62] ServerActionExecutor:458 - Task #62 failed to complete execution due to thrown exception: org.apache.ambari.server.AmbariException:The data directory has not been set.  Generated data can not be stored.
org.apache.ambari.server.AmbariException: The data directory has not been set.  Generated data can not be stored.
        at org.apache.ambari.server.serveraction.kerberos.PrepareDisableKerberosServerAction.execute(PrepareDisableKerberosServerAction.java:152)
        at org.apache.ambari.server.serveraction.ServerActionExecutor$Worker.execute(ServerActionExecutor.java:516)
        at org.apache.ambari.server.serveraction.ServerActionExecutor$Worker.run(ServerActionExecutor.java:453)
        at java.lang.Thread.run(Thread.java:748)  
ERROR [ambari-action-scheduler] ActionScheduler:447 - Operation completely failed, aborting request id: 30*

*Workaround:*

Try terminating the cluster again.

____________________________
