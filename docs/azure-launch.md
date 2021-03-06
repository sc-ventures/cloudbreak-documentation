## Launching Cloudbreak on Azure

These steps describe how to launch Cloudbreak on Azure for production. 
Before launching Cloudbreak on Azure, review and meet the [prerequisites](#prerequisites). Next, follow the steps below.  


{!docs/common/vm-req.md!}

{!docs/common/vm-launch.md!}


{!docs/common/azure-cred.md!}


#### Create interactive credential 

{!docs/common/azure-cred-int.md!}


#### Create app-based credential



Follow these steps to create an app based Cloudbreak credential.

[Comment]: <> (Hortonworks SE accounts do not have the permissions to perform this step. You must have the Owner role.)

**Prerequisites**

1. On Azure Portal, navigate to the **Active Directory** > **App Registrations** and register a new application. For more information, refer to [Create an Azure AD application](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal).

    > Aa an alternative to the steps listed below for creating an application registration, you use a utility called `azure-cli-tools`. The utility supports app creation and role assignment. It is available at [https://github.com/sequenceiq/azure-cli-tools/blob/master/cli_tools](https://github.com/sequenceiq/azure-cli-tools/blob/master/cli_tools).

    <a href="../images/cb_azure-appbased01.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-appbased01.png" width="650" title="Cloudbreak web UI"></a>  

1. Navigate to the **Subscriptions**, choose **Access control (IAM)**. Click **Add** and then assign the "Contributor" role to your newly created application by selecting "Contributor" under **Role** and typing your app name under **Select** (You must type your app name in order to find it):

    > This step typically requires admin permissions so you may have to contact your Azure administrator.

     <a href="../images/cb_azure-appbased03.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-appbased03.png" width="650" title="Azure Portal"></a>   


**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane.

2. Click **Create Credential**.

3. Under **Cloud provider**, select "Microsoft Azure".

4. Select **App based Login**:

    <a href="../images/cb_cb-azure-cred-app.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-azure-cred-app.png" width="650" title="Cloudbreak web UI"></a>

1. On the **Configure credential** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **App based**. |
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| Subscription Id | Copy and paste the Subscription ID from your **Subscriptions**. |
| Tenant Id | Copy and paste your Directory ID from your **Active Directory** > **Properties**. |
| App Id | Copy and paste the Application ID from your **Azure Active Directory** > **App Registrations** > your app registration's **Settings** > **Properties**. |
| Password | This is your application key. You can generate it from your **Azure Active Directory** app registration's **Settings** > **Keys**. |


    To obtain the **Subscription Id** from Subscriptions:

     <a href="../images/cb_azure-cred-subscription.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-cred-subscription.png" width="650" title="Cloudbreak web UI"></a>   

    To obtain the **App ID** (actually **Application ID**) and an application key from Azure Active Directory:

     <a href="../images/cb_azure-cred-app11.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-cred-app11.png" width="650" title="Cloudbreak web UI"></a>  

     <a href="../images/cb_azure-cred-app12.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-cred-app12.png" width="650" title="Cloudbreak web UI"></a>      

    To obtain the **Tenant ID** (actually **Directory Id**) from Azure Active Directory:

    <a href="../images/cb_azure-cred-directoryid.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-cred-directoryid.png" width="650" title="Cloudbreak web UI"></a>  


1. Click **Create**.

    > If you encounter errors, refer to [Troubleshooting Cloudbreak on Azure](trouble-azure.md).  

     Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to [create clusters](azure-create.md).

**Related links**  
[CLI tools](https://github.com/hortonworks/cloudbreak-azure-cli-tools/blob/master/cli_tools) (Hortonworks)    
[Use portal to create an Azure Active Directory application](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal) (External)     



<div class="next">
<a href="../azure-create/index.html">Next: Create a Cluster</a>
</div>
