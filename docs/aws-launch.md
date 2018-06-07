## Launching Cloudbreak on AWS

Before launching Cloudbreak on AWS, review and meet the AWS-specific [prerequisites](aws-pre.md).  
        
    
{!docs/common-general-prod/vm-req.md!}

{!docs/common-general-prod/vm-launch.md!}



### Create Cloudbreak credential

Before you can start creating clusters, you must first create a [Cloudbreak credential](concepts.md#cloudbreak-credential). Without this credential, you will not be able to create clusters via Cloudbreak. 

As part of the [prerequisites](aws-pre.md#authentication), you had two options to allow Cloudbreak to authenticate with AWS and create resources on your behalf: key-based or role-based authentication. Depending on your choice, you must configure a key-based or role-based credential: 

* [Create key-based credential](#create-key-based-credential)  
* [Create role-based credential](#create-role-based-credential)


#### Create key-based credential

{!docs/common-aws/aws-cred-key.md!} 
 

#### Create role-based credential

To perform these steps, you must know the **IAM Role ARN** corresponding to the "CredentialRole" (configured as a [aws-pre.md#prerequisite](#authentication)). 

> In case you have not created the CredentialRole yet, follow the steps in [Create CredentialRole](aws-pre.md#create-credentialrole).  

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

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to [create clusters](aws-create.md). 
      

<div class="next">
<a href="../aws-create/index.html">Next: Create a Cluster</a>
</div>

