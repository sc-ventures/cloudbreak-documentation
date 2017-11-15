## Release Notes


### 2.1.0 TP 

This release is technical preview: it is not suitable for production environments.

____________________________

#### New Features
____________________________

##### New UI/UX

Cloudbreak 2.1.0 TP introduces a new user interface.


##### New CLI

Cloudbreak 2.1.0 TP introduces a new CLI tool. For more information, refer to the [Install CLI](cli-install.md) and [CLI Reference](cli-reference.md) documentation. 


____________________________

#### Behavioral Changes
____________________________

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

____________________________

#### Known Issues
____________________________

##### (RMP-10114) Auto-scaling Is Not Available

Auto-scaling functionality is not available in Cloudbreak 2.1.0 TP. 
____________________________


##### (BUG-91543) Networks With No Subnets Are Not Supported 

You cannot create a cluster using an existing network that does not have any subnets. You must use a network that includes at least one subnet. If you try to use a network with no subnets, the cluster fails with the following error:   

*Infrastructure creation failed. Reason: Invalid value for field 'resource.network': 'https://www.googleapis.com/compute/v1/projects/siq-haas/global/networks/cbd-test'. A subnet mode Network must be specified for Subnetwork creation.: [ resourceType: GCP_SUBNET, resourceName: testgc-20171110211021 ]*
  
*Workaround*: Do not use this option. It will be removed in a future release.  
____________________________


##### (BUG-90848) "Do Not Use Security Group" Does Not Work for New Network 

In the *Network* section of the create cluster wizard, when you select to create a new network and subnet, there is a security group option "Do Not Use Security Group". This option does not work wen using a new network and subnet: if you select it, new security groups are created. This option will be removed in a future release.
____________________________


##### (BUG-90985) OpenStack Cluster Creation Fails at Network Configuration

When creating a cluster on OpenStack, if you do not select an existing network and subnet, the cluster fails with an error similar to:  
*Infrastructure creation failed. Reason: At least one of the following properties must be specified: network, network_id.*   
*Infrastructure creation failed. Reason: The Parameter (router_id) was not provided.*

*Workaround*: When creating a cluster on OpenStack, you must select an existing network and subnet. 
____________________________


##### (BUG-91077) Nodes Are Unhealthy After Sync

If your blueprint contains Druid, cluster node status may change to "unhealthy" after synchronizing with the cloud provider using the "Sync" option.    
*Workaround*: Manually start Druid by using Ambari web UI.  
____________________________


##### (BUG-91013) Incorrect Node Status After Cluster Restart 

You may sporadically experience an issue where after you stop and restart a cluster, the node status displayed in the "Hardware" section is incorrect.   

[comment]: <> (Not sure what the workaround is for BUG-91013?)
____________________________


##### (BUG-91071) Syntax Error During Cluster Downscale

When trying to downscale your cluster below the minimum required number of nodes, you may get the following error: *SyntaxError: Unexpected end of JSON input*.
 
*Workaround*: 

Check the *Event History* for more information:  

* If you tried to scale the cluster below the minimum required number of nodes, you will see: *New node(s) could not be removed from the cluster. Reason There is not enough node to downscale. Check the replication factor and the ApplicationMaster occupation.* It may take a few minutes for this message to appear in the the *Event History*.   
* In other cases, you should see a message informing you that downscale was successful. It may take a few minutes for this message to appear in the the *Event History*.  
____________________________


##### (BUG-91701) Cluster Resize Is Unclear 

If you try to resize your cluster, you may notice that the cluster name and cluster information in the text above the scaling controls are incorrect. 

*Workaround*:

* Ignore the "shared-services-demo2" mention and other information in the text above the scaling controls.  
* Note that by default the controls in the resize dialog are set to the *current* number of nodes. So if your cluster currently has 1 master and 5 worker nodes, the controls will be set to these values by default. To resize, adjust to the desired number of nodes.        
* Do not resize the Ambari node host group.  
 ____________________________
 

##### (BUG-91674) Network and Subnet Listed as N/A

When creating a new network and subnet for your cluster, the network and subnet information is unavailable on the cluster details page, showing "N/A".

*Workaround*: If you want to check which network and subnet are used for your cluster, navigate to the cloud provider account and find the cluster instances that were created for your cluster. Next, check which virtual network and subnet they are associated with. The steps vary depending on the provider.  
____________________________


[Comment]: <> (How about BUG-91699? Default Master security group ports are too open)

