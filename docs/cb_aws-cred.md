
## Create Cloudbreak credential

Before you can start creating clusters, you must first create a [Cloudbreak credential](https://hortonworks.github.io/cloudbreak-documentation/latest/). Without this credential, you will not be able to create clusters via Cloudbreak. 

As part of the prerequisites, you had two options to allow Cloudbreak to authenticate with AWS and create resources on your behalf: key-based or role-based [authentication](https://hortonworks.github.io/cloudbreak-documentation/latest/). Depending on your choice, you must configure a key-based or role-based credential.


### Create key-based credential

Follow these steps to create a key-based Cloudbreak credential. 


**Prerequisites**

In order to use the key-based Cloudbreak credential: 

* You must have an access key and secret key. For information on how to generate it, refer to [Use key-based authentication](https://hortonworks.github.io/cloudbreak-documentation/latest/).  

* Your AWS user must have the minimum permissions described in [Create CredentialRole](https://hortonworks.github.io/cloudbreak-documentation/latest/) as well as the permission to create an IAM role. 

If you are unable to obtain these permissions for your AWS user, you must use [role-based authentication](https://hortonworks.github.io/cloudbreak-documentation/latest/) instead of key-based authentication. If you would like to review both options, refer to [Authentication](https://hortonworks.github.io/cloudbreak-documentation/latest/). 
    
**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Amazon Web Services":

    <img src="../images/cb_cb-aws-cred-key.png" width="650" title="Cloudbreak web UI">  

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **Key Based**. | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| Access Key | Paste your access key. |
| Secret Access Key | Paste your secret key. |
 
4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

Congratulations! You've successfully launched Cloudbreak and create a Cloudbreak credential. Now you can start creating clusters. 
    
 

### Create role-based credential

Follow these steps to create a role-based Cloudbreak credential. 

**Prerequisites**

To perform these steps, you must know the **IAM Role ARN** corresponding to the "[CredentialRole](https://hortonworks.github.io/cloudbreak-documentation/latest/)" (configured as part of the prerequisites).   

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Amazon Web Services":

    <a href="../images/cb_cb-aws-cred-role.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-aws-cred-role.png" width="650" title="Cloudbreak web UI"></a>  

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **Role Based** (default value). | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| IAM Role ARN | Paste the IAM Role ARN corresponding to the "CredentialRole" that you created earlier. For example `arn:aws:iam::315627065446:role/CredentialRole` is a valid IAM Role ARN. |


4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to create clusters. 
      

