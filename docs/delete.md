## Delete Resources

In general, you can delete clusters from the Cloudbreak UI. If needed, you can also delete the cluster resources manually via the cloud provider tools. 

### Deleting Resources on AWS

TBD

### Deleting Resources on Azure

#### Deleting Clusters on Azure

You can delete clusters from the Cloudbreak UI. If needed, you can also delete the cluster manually by deleting the whole resource group created when the cluster was deployed. 

The name of the resource group, under which the cluster-related resources are organized always includes the name of the cluster, so you should be able to find the group by searching for that name in the **Resource groups**.

#### Delete Cloudbreak on Azure

To delete Cloudbreak, delete the whole related resource group.

**Steps**

1. From the Microsoft Azure Portal dashboard, select <img src="../images/resource-icon.png" width="150" title="Icon">.
2. Find the resource group that you want to delete, click on **...** and select **Delete**:

    <a href="../images/resource-delete.png" target="_blank" title="click to enlarge"><img src="../images/resource-delete.png" width="650" title="Azure Portal"></a>  

3. Type the name of the resource group to delete and click **Delete**.

### Deleting Resources on GCP

TBD

### Deleting Resources on OpenStack

TBD
