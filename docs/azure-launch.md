## Launching Cloudbreak on Azure

Before launching Cloudbreak on Azure, review and meet the prerequisites. Next, launch Cloudbreak using one of the two available methods. Once Cloudbreak is running, log in to the Cloudbreak UI and create a Cloudbreak credential.

### Meet the prerequisites

Before launching Cloudbreak on Azure, you must meet the following prerequisites.

#### Azure account

In order to launch Cloudbreak on the Azure, log in to your existing Microsoft Azure account. If you don't have an account, you can set it up at [https://azure.microsoft.com](https://azure.microsoft.com).

#### Azure region

Decide in which Azure region you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by Microsoft Azure](https://azure.microsoft.com/en-us/regions/).

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it.

**Related links**  
[Azure regions](https://azure.microsoft.com/en-us/regions/) (External)

#### Azure roles

In order to provision clusters on Azure, Cloudbreak must be able to assume a sufficient Azure role ("Owner" or "Contributor") via Cloudbreak credential:

* Your account must have the "[Owner](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#owner)" role (or a role with equivalent permissions) in the subscription in order to [create a Cloudbreak credential](#create-cloudbreak-credential) using the interactive credential method.

* Your account must have the "[Contributor](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#contributor)" role (or a role with equivalent permissions) in the subscription in order to [create a Cloudbreak credential](#create-cloudbreak-credential) using the app-based credential method. The role must be assigned to the app that you register in Cloudbreak.

To check the roles in your subscription, log in to your Azure account and navigate to **Subscriptions**.

**Related links**  
[Built-in roles: Owner](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#owner) (External)  
[Built-in roles: Contributor](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#contributor) (External)


#### SSH key pair

When launching Cloudbreak, you will be required to provide your public SSH key. If needed, you can generate a new SSH key pair:

* On MacOS X and Linux using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`  
* On Windows using [PuTTygen](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows)


### Launch Cloudbreak from a template 

Launch Cloudbreak deployer by using the following steps.

**Steps**

1. Log in to your [Azure Portal](https://portal.azure.com).

2. Click here to get started with Cloudbreak installation using the Azure Resource Manager template:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhortonworks%2Fazure-cbd-quickstart%2F2.5.0%2FmainTemplate.json" target="_blank"> ![deploy on azure](http://azuredeploy.net/deploybutton.png)</a>

3. The template for installing Cloudbreak will appear. On the **Basics** page, provide the following basic parameters:   

    > All parameters except "SmartSense Id" are required.

    **BASICS**

    | Parameter | Description |
|---|---|
| Subscription | Select which existing subscription you want to use. |
| Resource group | Select an existing resource group or create a new one by selecting **Create new** and entering a name for your new resource group. Cloudbreak resources will later be accessible in that chosen resource group. |
| Location | Select an Azure region in which you want to deploy Cloudbreak. |

    **SETTINGS**

    | Parameter | Description |
|---|---|
| Base Url | This is the URL to the page where the template is stored. |
| Location | This is an internal parameter. Do not change it.|
| Vm Size | Select virtual machine instance type to use for the Cloudbreak controller. The minimum instance type suitable for Cloudbreak is *Standard_DS3*. The minimum requirements are 16GB RAM, 40GB disk, 4 cores. |
| Admin Username | Create an admin login that you will use to log in to the Cloudbreak UI. Must be a valid email address. By default, admin@example.com is used but you should change it to your email address. |
| Admin User Password | Password for the admin login. Must be at least 8 characters containing letters, numbers, and symbols. |
| Username | Enter an admin username for the virtual machine. You will use it to SSH to the VM. By default, "cloudbreak" is used. |
| SmartSense | Select whether you want to use [SmartSense telemetry](smartsense.md). Default is "false" (not using SmartSense telemetry). |
| SmartSense Id | (Optional) Your SmartSense ID. |
| Remote Location |<p>Enter a valid [CIDR IP](http://www.ipaddressguide.com/cidr) or use one of the default tags. Default value is `Internet` which allows access from all IP addresses. Examples: </p><p><ul><li>10.0.0.0/24 will allow access from 10.0.0.0 through 10.0.0.255</li><li>'Internet' will allow access from all. This is not a secure option but you can use it it you are just getting started and are not planning to have the instance on for a longer period. </li><li>(Advanced) 'VirtualNetwork' will allow access from the address space of the Virtual Network.</li><li> (Advanced) 'AzureLoadBalancer' will allow access from the address space of the load balancer.</li></ul></p><p>For more information, refer to the [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).</p> |
| Ssh Key| <p>Paste your SSH public key.</p><p>You can use `pbcopy` to quickly copy it. For example: `pbcopy < /Users/homedir/.ssh/id_rsa.pub`</p> |
| Vnet New Or Existing | By default, Cloudbreak is launched in a new VNet called `cbdeployerVnet` and a new subnet called `cbdeployerSubnet`; if needed, you can customize the settings for the new VNet using available VNet and Subnet parameters. |
| Vnet Name | Provide the name for a new Vnet. Default is ``cbdeployerVnet`. |
| Vnet Subnet Name | Provide a name for a new subnet. Default is `cbdeployerSubnet`. |
| Vnet Address Prefix | Provide a CIDR for the virtual network. Default is `10.0.0.0/16`. |
| Vnet Subnet Address Prefix | Provide a CIDR for the subnet. Default is `10.0.0.0/24`. |
| Vnet RG Name | The name of the resource group in which the Vnet is located. If creating a new Vnet, enter the same resource group name as provided in the `Resource group` field in the **BASICS** section. |

4. Review terms of use and check "I agree to the terms and conditions stated above".

5. Click **Purchase**.

6. Your deployment should be initiated.  

    > If you encounter errors, refer to [Troubleshooting Cloudbreak on Azure](trouble-azure.md).   


**Related links**  
[CIDR IP](http://www.ipaddressguide.com/cidr) (External)   
[Filter network traffic with network security groups](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) (External)  


### Access Cloudbreak web UI

Log in to the Cloudbreak UI using the following steps.

**Steps**

1. When your deployment succeeds, you will receive a notification in the top-right corner. You can click on the link provided to navigate to the resource group created earlier.

    > This only works right after deployment. At other times, you can find your resource group by selecting **Resource Groups** from the service menu and then finding your resource group by name.

2. Once you've navigated to your resource group, click on **Deployments** and then click on **Microsoft.Template**:

    <a href="../images/cb_azure-resource-find.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-resource-find.png" width="650" title="Azure Portal"></a>

3. From **Outputs**, you can copy the link by clicking on the copy icon:

    <a href="../images/cb_azure-outputs.png" target="_blank" title="click to enlarge"><img src="../images/cb_azure-outputs.png" width="650" title="Azure Portal"></a>   

2. Paste the link in your browser's address bar.

{!docs/common/launch-access-ui.md!} 

3. Now you should be able to access Cloudbreak UI and log in with the **Admin email address** and **Admin password** that you created when launching Cloudbreak. 

4. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a>  

The last task that you need to perform before you can use Cloudbreak is to [create a cloudbreak credential](#create-cloudbreak-credential).         


### Create Cloudbreak credential

Before you can start creating clusters, you must first create a [Cloudbreak credential](architecture.md#cloudbreak-credential). Without this credential, you will not be able to create clusters via Cloudbreak. Cloudbreak works by connecting your Azure account through this credential, and then uses it to create resources on your behalf.

There are two methods for creating a Cloudbreak credential:


| Method | Description | Prerequisite | Steps |
|---|---|---|---|
| **Interactive** | The advantage of using this method is that the app and service principal creation and role assignment are fully automated, so the only input that you need to provide is the Subscription ID and Directory ID. During the interactive credential creation, you are required to log in to your Azure account. | (1) Your account must have the "Owner" role (or its equivalent) in the subscription. (2) You must be able log in to your Azure account. | To configure an interactive credential, refer to [Create an interactive credential](#create-an-interactive-credential). |
| **App-based** | The advantage of the app-based credential creation is that it allows you to create a credential without logging in to the Azure account, as long as you have been given all the information. In addition to providing your Subscription ID and  Directory ID, you must provide information for your previously created Azure AD application (its ID and key which allows access to it). | (1) Your account must have the "Contributor" role (or equivalent) in the subscription. (2) You or your Azure administrator must perform prerequisite steps of registering an Azure application and assigning the  "Contributor" role to it. This step typically requires admin permissions so you may have to contact your Azure administrator. | To configure an app based credential, refer to [Create an app-based credential](#create-an-app-based-credential). |


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
