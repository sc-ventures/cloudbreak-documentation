## Delete Resources

You must terminate all clusters associated with a Cloudbreak before you can terminate the Cloudbreak instance. In general, you should delete clusters from the Cloudbreak UI. If needed, you can also delete the cluster resources manually via the cloud provider tools. 

#### Delete Clusters  

The proper way to delete clusters is to use the the **Terminate** option available in the Cloudbreak UI. If the terminate process fails, try the **Terminate** > **Force terminate** option.

If the force termination does not delete all cluster resources, delete the resources manually:

* To find the VMs, click on the links available in the cluster details. 
* To find the network and subnet, see the **Cluster Information** in the cluster details. 
* On Azure, you can delete the cluster manually by deleting the whole resource group created when the cluster was deployed. The name of the resource group, under which the cluster-related resources are organized always includes the name of the cluster, so you should be able to find the group by searching for that name in the **Resource groups**.
  

### Delete Cloudbreak on AWS

If you want to delete the Cloudbreak instance, you can do so by deleting the EC2 instance on which it is running.

**Steps**

1. Log in to the AWS Management Console.

2. Browse to the EC2 Management Console.

3. navigate to **Instances**.

4. Select the instance that you want to delete and then select **Actions** > **Instance State** > **Terminate**.

5. Click **Yes, Terminate** to confirm.

    <a href="../images/aws-delete.png" target="_blank" title="click to enlarge"><img src="../images/aws-delete.png" width="650" title="Azure Portal"></a>  


### Delete Cloudbreak on Azure

You can delete Cloudbreak instance from your Azure account by deleting related resoucres. To delete a Cloudbreak instance:

* If you deployed Cloudbreak in a new resource group: to delete Cloudbreak, delete the whole related resource group.

* If you deployed Cloudbreak in an existing resource group: navigate to the group and delete only Cloubdreak-related resources such as the VM.


**Steps**

1. From the Microsoft Azure Portal dashboard, select **Resource groups**.

2. Find the resource group that you want to delete.

3. If you deployed Cloudbreak in a new resource group, you can delete the whole resource group. Click on **...** and select **Delete**:

    <a href="../images/azure-delete.png" target="_blank" title="click to enlarge"><img src="../images/azure-delete.png" width="650" title="Azure Portal"></a>  

    Next, type the name of the resource group to delete and click **Delete**.
    
4. If you deployed Cloudbreak in an existing resource group, navigate to the details of the resource group and delete only Cloudbreak-related resources such as the VM.    


### Delete Cloudbreak on GCP 

You can delete Cloudbreak instance from your Google Cloud account. 


**Steps**

1. Navigate to your Google Cloud account.

2. Navigate to **Compute Engine** > **VM instances**.

3. Select the instance that you want to delete:

    <a href="../images/gcp-delete.png" target="_blank" title="click to enlarge"><img src="../images/gcp-delete.png" width="650" title="Azure Portal"></a>     

4. Click on the delete icon and then confirm delete. 


### Delete Cloudbreak on OpenStack

You can delete Cloudbreak instance from your OpeenStack console. 

**Steps**

1. Navigate to your OpenStack account.

2. Navigate to **Instances**.

3. Select the instance to delete, click **Terminate Instances**, and confirm.
