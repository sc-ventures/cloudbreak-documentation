## Managing and Monitoring Your Clusters  

You can manage monitor your clusters from the Cloudbreak UI. To do that, click on the tile representing the cluster that you want to access: 

<a href="../images/cb-ui3.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui3.png" width="650" title="Azure Portal"></a> 


### Repairing Your Cluster

To trigger repair process for your cluster, click **repair**. Faulty nodes will be deleted from the cluster and new ones will be added in their place.


### Synchronizing with Cloud Provider

TBD
 

### Resizing Your Cluster

To resize a cluster, follow these steps.

**Steps**

1. Browse to the cluster details.

2. Click **CLUSTER ACTIONS** and select **Resize**. The cluster resize dialog is displayed.

3. Using the +/- control, you can adjust how many worker and compute nodes to add or remove from the cluster. 

4. Click **RESIZE CLUSTER** to initiate the scale-up/scale-down.


### Stopping and Restarting

TBD


### Terminating Your Cluster 

To terminate your cluster, click **terminate**. All cluster-related resources will be deleted, unless the network is used by other VMs, in which case it will not be deleted. 


### Viewing Cluster History

From the navigation menu in the Cloudbreak UI, you can access the History page that allows you to generate a report showing basic information related to the clusters that were running within the specified range of dates.

To generate a report, follow these steps.

**Steps**

1. From the Cloudbreak UI navigation menu, select **History**.

2. On the History page, select the range of dates and click **Show history** to generate a report for the selected period.

#### History Report Content 

>>>>TO-DO: How are these entries broken down? Is there one entry per instance group?

Each entry in the report represents one cluster instance group. For each entry, the report includes the following information:

* **Created** - The date when your cluster was created (YYYY-MM-DD).
* **Provider** - The name of the cloud provider (AWS, Azure, Google, or OpenStack) on which the cluster instances are/were running.
* **Cluster Name** - The name that you selected for the cluster.
* **Worker Count** - The number of worker nodes in the cluster. This number may be a decimal if a cluster has been resized.
* **Instance Type** - Provider-specific VM type of the cluster instances.
* **Instance Group** - The name of the instance group.  
* **Region** - The AWS region in which your cluster is running.
* **Running Time (hours)** - The sum of the running times for all the nodes in the instance group.

The **AGGREGATE RUNNING TIME** is the sum of the Running Times, adjusted for the selected time range.

To learn about how your cloud provider bills you for the VMs, refer to their documentation:

* [AWS](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-instance-hour-billing/)      
* [Azure](https://azure.microsoft.com/en-us/pricing/faq/virtual-machines-how-do-instance-sizes-get-billed/)     
* [GCP](https://cloud.google.com/compute/pricing)   

