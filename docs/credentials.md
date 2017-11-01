
##  Manage Cloudbreak Credentials 

You can view and manage Cloudbreak credentials in the **Credentials** tab by clicking **Create credential** and providing required parameters. You must create at least one credential in order to be able to create a cluster. 

<a href="../images/cb-credentials.png" target="_blank" title="click to enlarge"><img src="../images/cb-credentials.png" width="650" title="Cloudbreak web UI"></a> 

### Create Cloudbreak Credental  

For steps, refer to:

* [Create Credential on AWS](aws-launch.md#create-cloudbreak-credential)  
* [Create Credential on Azure](azure-launch.md#create-cloudbreak-credential)  
* [Create Credential on GCP](gcp-launch.md#create-cloudbreak-credential) 
* [Create Credential on OpenStack](os-launch.md#create-cloudbreak-credential)


### Set a Default Credential

If using multiple Cloudbreak credentials, you can select one credential and use it as default for creating clusters. This default credential will be pre-selected in the create cluster wizard.
 
To set a default credential:

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Click **Set as default** next to the credential that you would like to set as default.  
3. Click **Yes** to confirm. 

### Delete a Credential 

To delete a credential:

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Select one or more credentials by checking their corresponding checkboxes.
2. Click **Delete**. 
3. Click **Yes** to confirm. 
