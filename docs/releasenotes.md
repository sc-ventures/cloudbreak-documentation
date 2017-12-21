## Release Notes

### 2.3.0

Cloudbreak 2.3.0 is a general availability release, which is suitable for production deployments. 

____________________________

#### New Features
____________________________


##### Pre-termination Recipes

A new "pre-termination" recipe type is introduced to allow you to run custom scripts before cluster termination. Refer to updated [Recipes](recipes.md) documentation.  

____________________________

#### Behavioral Changes
____________________________


TBD

____________________________

#### Fixed Issues 
____________________________


The following issues have been fixed in this release: 

| Jira |  Description |
|---|---|
| BUG-93602 | "Do not create Public IPs" Azure cluster option was fixed and can now be used. |
| BUG-91071 | Removed "SyntaxError: Unexpected end of JSON input" error when trying to downscale a cluster below the minimum required number of nodes. |

*Workaround:*

Check the Event History for more information:

* If you tried to scale the cluster below the minimum required number of nodes, you will see: New node(s) could not be removed from the cluster. Reason There is not enough node to downscale. Check the replication factor and the ApplicationMaster occupation. It may take a few minutes for this message to appear in the the Event History.  
* In other cases, you should see a message informing you that downscale was successful. It may take a few minutes for this message to appear in the the Event History.  
____________________________



____________________________

#### Known Issues
____________________________

##### (RMP-10114) Auto-scaling Is Not Available

Auto-scaling functionality is not available in Cloudbreak 2.2.0 TP. 
____________________________



##### (RMP-10409) Azure Disk Encryption Is Not Available

Azure disk encryption functionality is not available in Cloudbreak 2.2.0 TP.
____________________________


##### (BUG-93556) Hadoop YARN Provider is not Supported 

When creating a Cloudbreak credential, the UI shows a "Hadoop YARN" cloud provider as an option.

*Workaround*: 

Do not use this option. 
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

