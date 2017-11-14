## Release Notes


### 2.1.0 TP 

This release is technical preview: it is not suitable for production environments.

#### New Features

##### New UI/UX

Cloudbreak 2.1.0 TP introduces a new user interface.



##### New CLI

Cloudbreak 2.1.0 TP introduces a new CLI tool. For more information, refer to the [Install CLI](cli-install.md) and [CLI Reference](cli-reference.md) documentation. 

____________________________


#### Behavioral Changes

##### Creating Custom Images 

The functionality which enables you to create custom images was changed and improved. Refer to  [Custom Images](images.md).


##### Removal of Cloudbreak Shell 

Cloudbreak Shell is no longer available in Cloudbreak 2.1.0 TP and later. It was replaced by the [Cloudbreak CLI](cli-install.md).


##### Removal of Platforms 

The [platforms](http://hortonworks.github.io/cloudbreak-docs/release-1.16.4/topologies/) feature was removed. 


##### Removal of Mesos 

Cloudbreak 2.1.0 TP does not support Mesos cloud provider.


##### Removal of Templates

Earlier versions of Cloudbreak allowed you to save infrastructure, network, and security group templates. This feature was removed. Instead, you can define VMs, storage, networks, and security groups as part of the create cluster wizard. 

____________________________________

#### Known Issues

##### (RMP-10114) Auto-scaling Is Not Available

Auto-scaling functionality is not available in Cloudbreak 2.1.0 TP. 


##### (BUG-91543) Networks With No Subnets Are Not Supported 

You cannot create a cluster using an existing network that does not have any subnets. You must use a network that includes at least one subnet. If you try to use a network with no subnets, the cluster fails with the following error:

```
Infrastructure creation failed. Reason: Invalid value for field 'resource.network': 'https://www.googleapis.com/compute/v1/projects/siq-haas/global/networks/cbd-test'. A subnet mode Network must be specified for Subnetwork creation.: [ resourceType: GCP_SUBNET, resourceName: testgc-20171110211021 ]
```  
*Workaround*: Do not use this option. It will be removed in a future release.  



##### (BUG-90848) "Do Not Use Security Group" Does Not Work for New Network 

In the *Network* section of the create cluster wizard, when you select to create a new network and subnet, there is a security group option "Do Not Use Security Group". This option does not work wen using a new network and subnet: if you select it, new security groups are created. This option will be removed in a future release.



##### (BUG-91071) Syntax Error During Cluster Downscale

When trying to downscale your cluster below the minimum required number of nodes, you may get the following error:
 
```
SyntaxError: Unexpected end of JSON input
```

The log provides more information on the error: `New node(s) could not be removed from the cluster. Reason There is not enough node to downscale. Check the replication factor and the ApplicationMaster occupation.`  
*Workaround*: Do not scale the cluster below the minimum required number of nodes.  


##### (BUG-91077) Nodes Are Unhealthy After Sync

If your blueprint contains Druid, cluster node status may change to "unhealthy" after synchronizing with the cloud provider using the "Sync" option.    
*Workaround*: Manually start Druid by using Ambari web UI.  



##### (BUG-91013) Incorrect Node Status After Cluster Restart 

You may sporadically experience an issue where after you stop and restart a cluster, the node status displayed in the "Hardware" section is incorrect.   

[comment]: <> (Not sure what the workaround is for BUG-91013?)

 

##### (BUG-90985) OpenStack Cluster Creation Fails at Network Configuration

When creating a cluster on OpenStack, if you do not select an existing network and subnet, the cluster fails with an error similar to:

```
Infrastructure creation failed. Reason: At least one of the following properties must be specified: network, network_id.
```
or

```
Infrastructure creation failed. Reason: The Parameter (router_id) was not provided.
```

*Workaround*: When creating a cluster on OpenStack, you must select an existing network and subnet. 



[Comment]: <> (How about BUG-91699? Default Master security group ports are too open?)


[Comment]: <> (Removed "BUG-91540 Disabled Buttons in Create Cluster")
 
