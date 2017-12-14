## Managing and Monitoring Clusters  

You can manage monitor your clusters from the Cloudbreak UI. To do that, click on the tile representing the cluster that you want to access. The actions available for your cluster are listed in the top right corner: 

<a href="../images/cb-ui2.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui2.png" width="650" title="Cloudbreak web UI"></a> 


<div class="note">
  <p class="first admonition-title">Tips</p>
  <p class="last"><ul>
  <li>To add or remove nodes from your cluster click <b>ACTIONS>Resize</b>.</li>
  <li>To synchronize your cluster with the cloud provider account click <b>ACTIONS>Sync</b>.</li>
  <li>To temporarily stop your cluster click <b>STOP</b>.</li>
  <li>To terminate your cluster click <b>TERMINATE</b>.</li>
</ul>
</p>
</div>


### Resize Cluster

To resize a cluster, follow these steps.

**Steps**

1. Browse to the cluster details.

2. Click **Actions** and select **Resize**. The cluster resize dialog is displayed.

3. Using the +/- controls, adjust the number of nodes for a chosen host group. 

    > You can only modify one host group at a time.   
    > It is not possible to resize the Ambari server host group.     

4. Click **Yes** to confirm the scale-up/scale-down.

    While nodes are being added or removed, cluster status changes to "Update In Progress". Once the operation has completed, cluster status changes back to "Running". Messages similar to the following are written to the "Event History", : 

    <pre>Ambari cluster scaled up
11/6/2017, 12:33:40 PM
Scaling up the Ambari cluster
11/6/2017, 12:26:59 PM
Stack successfully upscaled
11/6/2017, 12:26:54 PM
Bootstrapping new nodes
11/6/2017, 12:26:16 PM
Infrastructure metadata extension finished
11/6/2017, 12:26:10 PM
Billing changed due to upscaling of cluster infrastructure
11/6/2017, 12:26:10 PM
Adding 1 new instances to the infrastructure
11/6/2017, 12:25:23 PM</pre>


### Synchronize with Cloud Provider

If you have just made changes on your cloud provider side (for example, deleted cluster VMs) and you would like to synchronize Cloudbreak with the cloud provider, use the **sync** option. 

[comment]: <> (What are some examples where this option should be used?)

To synchronize your cluster with the cloud provider, follow these steps. 

**Steps**

1. Browse to the cluster details.

2. Click **Actions** and select **Sync**.
 
3. Click **Yes** to confirm.

    Your cluster infrastructure is synchronized based on changes on the cloud provider. The updates are written to the "Event History". 
 
 
### Stop Cluster 

Cloudbreak supports stopping and restarting clusters. To stop and restart a cluster managed by Cloudbreak, use the options available from the Coudbreak UI. 

**Steps**

1. Browse to the cluster details.
 
2. Click **Stop** to stop a currently running cluster.  

3. Click **Yes** to confirm. 

    Your cluster status changes to "Stopping in progress" and then to "Stopped". You should see the following messages in the "Event History":

    <pre>Billing stopped, the cluster and its infrastructure have been terminated
11/6/2017, 12:46:48 PM
Infrastructure successfully stopped
11/6/2017, 12:46:47 PM
Infrastructure is now stopping
11/6/2017, 12:43:58 PM
Ambari cluster stopped
11/6/2017, 12:43:52 PM
Ambari services have been stopped.
11/6/2017, 12:43:49 PM
Stopping Ambari services.
11/6/2017, 12:42:14 PM
Stopping Ambari cluster
11/6/2017, 12:42:10 PM
Cluster infrastructure stop requested
11/6/2017, 12:42:06 PM</pre>

Once stopping the infrastructure has completed, you will see a **Start** option to restart your cluster. 


### Restart Cluster 

If your cluster is in the "Stopped" state, you can restart the cluster by follow these steps.

**Steps**

1. click **Start**. This option is only available when the cluster has been stopped. 

2. Click **Yes** to confirm.

    Your cluster status changes to "Start in progress" and then to "Running". You should see the following messages in the "Event History":

    <pre>Ambari cluster started; Ambari ip:35.203.149.236
11/6/2017, 1:02:13 PM
Ambari services have been started.
11/6/2017, 1:02:08 PM
Starting Ambari services.
11/6/2017, 12:54:37 PM
Starting Ambari cluster
11/6/2017, 12:52:57 PM
Billing started, the cluster and its infrastructure have successfully been started
11/6/2017, 12:52:48 PM
Infrastructure successfully started
11/6/2017, 12:52:48 PM
Infrastructure is now starting
11/6/2017, 12:52:26 PM
Ambari cluster start requested
11/6/2017, 12:52:24 PM</pre>


### Terminate Cluster 

To terminate a cluster managed by Cloudbreak, use the option available from the Coudbreak UI. 

**Steps**

1. Browse to the cluster details.
 
2. Click **Terminate**. 

3. Click **Yes** to confirm.

    All cluster-related resources will be deleted, unless the resources (such as networks and subnets) existed prior to cluster creation or are used by other VMs in which case they will be preserved. 


#### Force Terminate

Cluster deletion may fail if Cloudbreak is unable to delete one or more of the cloud resources that were part of your cluster infrastructure. In such as case, you can use the **Terminate** > **Force terminate** option to remove the cluster entry from the Cloudbreak web UI, but you must also check your cloud provider account to see if there are any resources that must be deleted manually.

**Steps**

1. Browse to the cluster details.
 
2. Click **Terminate**. 

3. Check  **Force terminate**.

3. Click **Yes** to confirm. 

    When terminating a cluster with Kerberos enabled, you have an option to disable Kerberos prior to cluster termination. This option removes any cluster-related principals from the KDC.

4. This deletes the cluster tile from the UI.  

4. Log in to your cloud provider account and [manually delete](cb-delete.md) any resources that failed to be deleted.



### View Cluster History

From the navigation menu in the Cloudbreak UI, you can access the History page that allows you to generate a report showing basic information related to the clusters that were running within the specified range of dates.

To generate a report, follow these steps.

**Steps**

1. From the Cloudbreak UI navigation menu, select **History**.

2. On the History page, select the range of dates and click **Show History** to generate a report for the selected period.

    <a href="../images/cb-history.png" target="_blank" title="click to enlarge"><img src="../images/cb-history.png" width="650" title="Cloudbreak web UI"></a> 


#### History Report Content 

Each entry in the report represents one cluster instance group. For each entry, the report includes the following information:

* **Created** - The date when your cluster was created (YYYY-MM-DD).
* **Provider** - The name of the cloud provider (AWS, Azure, Google, or OpenStack) on which the cluster instances are/were running.
* **Cluster Name** - The name that you selected for the cluster.  
* **Instance Group** - The name of the host group.   
* **Instance Count** - The number of nodes in the host group. This number may be a decimal if a cluster has been resized.
* **Instance Type** - Provider-specific VM type of the cluster instances. 
* **Region** - The AWS region in which your cluster is/was running.
* **Availability Zone** - The availability zone in which your cluster is/was running.      
* **Running Time (hours)** - The sum of the running times for all the nodes in the instance group.

The **AGGREGATE RUNNING TIME** is the sum of the Running Times, adjusted for the selected time range.

To learn about how your cloud provider bills you for the VMs, refer to their documentation:

* [AWS](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-instance-hour-billing/)      
* [Azure](https://azure.microsoft.com/en-us/pricing/faq/virtual-machines-how-do-instance-sizes-get-billed/)     
* [GCP](https://cloud.google.com/compute/pricing)   

