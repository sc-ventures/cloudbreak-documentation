## Launching Cloudbreak on Azure

Before launching Cloudbreak on Azure:

* Review and meet the [prerequisites](#prerequisites).  
* If you haven't already, review the [deployment options](deployment-options.md).  

The steps below describe how to launch Cloudbreak by using one of the two available deployment options (quickstart and production), access Cloudbreak web UI, and create a Cloudbreak credential.


### (Production) Launch Cloudbrak on your own VM

Refer to [Launch Cloudbreak on your own VM](vm-launch.md). 


### Create Cloudbreak credential

Before you can start creating clusters, you must first create a [Cloudbreak credential](architecture.md#cloudbreak-credential). Without this credential, you will not be able to create clusters via Cloudbreak. Cloudbreak works by connecting your Azure account through this credential, and then uses it to create resources on your behalf.

There are two methods for creating a Cloudbreak credential:


| Method | Description | Prerequisite | Steps |
|---|---|---|---|
| **Interactive** | The advantage of using this method is that the app and service principal creation and role assignment are fully automated, so the only input that you need to provide is the Subscription ID and Directory ID. During the interactive credential creation, you are required to log in to your Azure account. | (1) Your account must have the "Owner" role (or its equivalent) in the subscription. (2) You must be able log in to your Azure account. | To configure an interactive credential, refer to [Create an interactive credential](#create-an-interactive-credential). |
| **App-based** | The advantage of the app-based credential creation is that it allows you to create a credential without logging in to the Azure account, as long as you have been given all the information. In addition to providing your Subscription ID and  Directory ID, you must provide information for your previously created Azure AD application (its ID and key which allows access to it). | (1) Your account must have the "Contributor" role (or equivalent) in the subscription. (2) You or your Azure administrator must perform prerequisite steps of registering an Azure application and assigning the "Contributor" role to it. This step typically requires admin permissions so you may have to contact your Azure administrator. | To configure an app based credential, refer to [Create an app-based credential](#create-an-app-based-credential). |


#### Create an interactive credential

Follow these steps to create an interactive Cloudbreak credential.

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane.

2. Click **Create Credential**.

3. Under **Cloud provider**, select "Microsoft Azure".

4. Select **Interactive Login**:

    <a href="../images/cb_cb-azure-cred-inter.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-azure-cred-inter.png" width="650" title="Cloudbreak web UI"></a>     

5. Provide the following information:

    | Parameter | Description |
|---|---|
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| Subscription Id | Copy and paste the Subscription ID from your **Subscriptions**. |
| Tenant Id | Copy and paste your Directory ID from your **Active Directory** > **Properties**. |
| Azure role type | <p>You have the following options:<ul><li>"Use existing Contributor role" (default): If you select this option, Cloudbreak will use the "[Contributor](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#contributor)" role to create resources. This requires no further input.</li><li>"Reuse existing custom role": If you select this option and enter the name of an existing role, Cloudbreak will use this role to create resources.</li><li>"Let Cloudbreak create a custom role": If you select this option and enter a name for the new role, the role will be created. When choosing role name, make sure that there is no existing role with the name chosen. For information on creating custom roles, refer to [Azure](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles) documentation. </li></ul></p><p>If using a custom role, make sure that it includes the necessary Action set for Cloudbreak to be able to manage clusters: `Microsoft.Compute/*`, `Microsoft.Network/*`, `Microsoft.Storage/*`, `Microsoft.Resources/*`.</p> |

    To obtain the **Subscription Id**:

     <a href="../images/cb_azure-cred-subscription.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-cred-subscription.png" width="650" title="Cloudbreak web UI"></a>   

    To obtain the **Tenant ID** (actually **Directory Id**):

    <a href="../images/cb_azure-cred-directoryid.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-cred-directoryid.png" width="650" title="Cloudbreak web UI"></a>    

6. After providing the parameters, click **Interactive Login**.

6. Copy the code provided in the UI:

    <a href="../images/cb_cb-azure-cred-inter2.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-azure-cred-inter2.png" width="650" title="Cloudbreak web UI"></a>     

7. Click **Azure login** and a new **Device login** page will open in a new browser tab:

     <a href="../images/cb_azure-device-login.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-device-login.png" width="650" title="Azure Portal"></a>  

8. Next, paste the code in field on the  **Device login** page and click **Continue**.
9. Confirm your account by selecting it:

     <a href="../images/cb_azure-device-login2.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-device-login2.png" width="650" title="Azure Portal"></a>

10. A confirmation page will appear, confirming that you have signed in to the Microsoft Azure Cross-platform Command Line Interface application on your device. You may now close this window.

    > If you encounter errors, refer to [Troubleshooting Azure](trouble-azure.md).

     Congratulations! You've successfully launched and configured Cloudbreak. Now you can use Cloudbreak to [create clusters](azure-create.md).


#### Create an app-based credential

Follow these steps to create an app based Cloudbreak credential.

[Comment]: <> (Hortonworks SE accounts do not have the permissions to perform this step. You must have the Owner role.)

**Prerequisites**

1. On Azure Portal, navigate to the **Active Directory** > **App Registrations** and register a new application. For more information, refer to [Create an Azure AD application](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal).

    > Aa an alternative to the steps listed below for creating an application registration, you use a utility called `azure-cli-tools`. The utility supports app creation and role assignment. It is available at [https://github.com/sequenceiq/azure-cli-tools/blob/master/cli_tools](https://github.com/sequenceiq/azure-cli-tools/blob/master/cli_tools).

    <a href="../images/cb_azure-appbased01.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-appbased01.png" width="650" title="Cloudbreak web UI"></a>  

1. Navigate to the **Subscriptions**, choose **Access control (IAM)**. Click **Add** and then assign the "Contributor" role to your newly created application by selecting "Contributor" under **Role** and your app name under **Select**:

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
