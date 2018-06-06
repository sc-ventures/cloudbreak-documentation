**Prerequisites**

To perform these steps, you must know your access and secret key. If needed, you or your AWS administrator can generate new access and secret keys from the **IAM Console** > **Users** > select a user > **Security credentials**:

<a href="../images/cb_aws-iam_security_creds.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws-iam_security_creds.png" width="650" title="IAM Console"></a>  

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Amazon Web Services":

    <a href="../images/cb_cb-aws-cred-key.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-aws-cred-key.png" width="650" title="Cloudbreak web UI"></a>  

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

    
    Congratulations! You've successfully launched Cloudbreak and create a Cloudbreak credential. Now it's time to [create a cluster](aws-create.md). 