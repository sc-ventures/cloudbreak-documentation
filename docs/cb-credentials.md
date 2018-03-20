
## Managing Cloudbreak Credentials 

You can view and manage Cloudbreak credentials in the **Credentials** tab by clicking **Create credential** and providing required parameters. You must create at least one credential in order to be able to create a cluster. 


### Create Cloudbreak Credential  

For steps, refer to:

* [Create Credential on AWS](aws-launch.md#create-cloudbreak-credential)  
* [Create Credential on Azure](azure-launch.md#create-cloudbreak-credential)  
* [Create Credential on GCP](gcp-launch.md#create-cloudbreak-credential) 
* [Create Credential on OpenStack](os-launch.md#create-cloudbreak-credential)


### View Credential Details

To view credential details, follow these steps.

**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Click on the name of a credential. 

### Modify an Existing Credential

You can modify an existing Cloudbreak credential by following these steps.

> Credential name cannot be changed.  
> Sensitive parameter values will not be displayed and you will have to reenter them.    

**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2.Click on <img src="../images/edit.png" alt="On" />  next to the credential that you want to edit.  
3. When done making changes, click **Save** to save your changes.  



### Set a Default Credential

If using multiple Cloudbreak credentials, you can select one credential and use it as default for creating clusters. This default credential will be pre-selected in the create cluster wizard.
 
**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Click **Set as default** next to the credential that you would like to set as default.  
3. Click **Yes** to confirm. 


### Delete a Credential 

To delete a credential, follow these steps.

**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Select one or more credentials by checking their corresponding checkboxes.
2. Click **Delete**. 
3. Click **Yes** to confirm. 


