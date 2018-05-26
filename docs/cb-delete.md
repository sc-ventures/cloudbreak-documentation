## Deleting Cloudbreak

You must terminate all clusters associated with a Cloudbreak before you can terminate the Cloudbreak instance. In general, you should delete clusters from the Cloudbreak UI. If needed, you can also delete the cluster resources manually via the cloud provider tools. 

### Deleting clusters  

The proper way to delete clusters is to use the the **Terminate** option available in the Cloudbreak UI. If the terminate process fails, try the **Terminate** > **Force terminate** option.

If the force termination does not delete all cluster resources, delete the resources manually:

* To find the VMs, click on the links available in the cluster details. 
* To find the network and subnet, see the **Cluster Information** in the cluster details. 
* On Azure, you can delete the cluster manually by deleting the whole resource group created when the cluster was deployed. The name of the resource group, under which the cluster-related resources are organized always includes the name of the cluster, so you should be able to find the group by searching for that name in the **Resource groups**.

Upon cluster termination, Cloudbreak only terminates the resources that it created. It does not terminate any resources (such as networks, subnets, roles, and so on) which existed prior to cluster creation. 
  

### Delete Cloudbreak on AWS

If you want to delete the Cloudbreak instance, you can do so by deleting the EC2 instance on which it is running.

**Steps**

1. Log in to the CloudFormation Console.

1. Select the deployment that you want to delete.
 
1. select **Actions** > **Delete Stack**.

1. Click **Yes, Delete** to confirm.
 
 All resources created as part of this stack (such as the Cloudbreak VM) will be deleted. 


### Delete Cloudbreak on Azure

You can delete Cloudbreak instance from your Azure account by deleting related resources. To delete a Cloudbreak instance:

* If you deployed Cloudbreak in a new resource group: to delete Cloudbreak, delete the whole related resource group.

* If you deployed Cloudbreak in an existing resource group: navigate to the group and delete only Cloudbreak related resources such as the VM.


**Steps**

1. From the Microsoft Azure Portal dashboard, select **Resource groups**.

2. Find the resource group that you want to delete.

3. If you deployed Cloudbreak in a new resource group, you can delete the whole resource group. Click on **...** and select **Delete**:

    <a href="../images/cb_azure-delete.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-delete.png" width="650" title="Azure Portal"></a>  

    Next, type the name of the resource group to delete and click **Delete**.
    
4. If you deployed Cloudbreak in an existing resource group, navigate to the details of the resource group and delete only Cloudbreak-related resources such as the VM.    


### Delete Cloudbreak on GCP 

There are two ways to delete a previously created Cloudbreak deployment from your Google Cloud account. 

(Option 1) You can delete the deployment from the Google Cloud console in your browser, from the **Deployment Manager > Deployments**:

<a href="../images/cb_gcp-delete.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-delete.png" width="650" title="GCP"></a>

(Option 2) You can delete the deployment by using the following gcloud CLI command:

<pre>gcloud deployment-manager deployments delete deployment-name -q</pre>

For example: 
    
<pre>gcloud deployment-manager deployments delete cbd-deployment -q</pre> 


### Delete Cloudbreak on OpenStack

You can delete Cloudbreak instance from your OpenStack console. 

**Steps**

1. Navigate to your OpenStack account.

2. Navigate to **Instances**.

3. Select the instance to delete, click **Terminate Instances**, and confirm.
