## Launch Cloudbreak on Azure


### Meet the Prerequisites 

Before launching Cloudbreak on Azure, you must meet the following prerequisites.

#### Azure Account 

In order to launch Cloubdreak on the Azure Marketplace, you need to log in to your existing Microsoft Azure account. If you don't have an account, you can set it up at [https://azure.microsoft.com](https://azure.microsoft.com).

#### Azure Roles 

In order to provision clusters on Azure, Cloudbreak must be able to assume a sufficient Azure role ("Owner" or "Contributor") via Cloudbreak credential: 

* Your account must have the "[Owner](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#owner)" role in the subscription in order to [create a Cloudbreak credential](#create-cloudbreak-credential) using the interactive credential method. 

* Your account must have the "[Contributor](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#contributor)" role (or higher) in the subscription in order to [create a Cloudbreak credential](#create-cloudbreak-credential) using the app-based credential method. 

To check the roles that your subscription has, in your Azure account navigate to **Subscriptions**. 

#### Azure Region

Decide in which Azure region you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by Microsoft Azure](https://azure.microsoft.com/en-us/regions/). 

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it.

#### SSH Key Pair

When launching Cloudbreak, you will be required to provide your public SSH key. If needed, you can generate a new SSH keypair using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`


### Launch Cloudbreak 

You have two options to launch Cloudbreak on Azure:

* Launch via Azure Resource Manager Template
* Launch from Azure Marketplace (Technical Preview)


#### (Option 1) Launch via Azure Resource Manager Template

1. Log in to your [Azure Portal](https://portal.azure.com).

2. Click here to get started with Cloudbreak installation using the Azure Resource Manager template:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsequenceiq%2Fazure-cbd-quickstart%2F1.16.1%2FmainTemplate.json" target="_blank"> ![deploy on azure](http://azuredeploy.net/deploybutton.png)</a>
    
3. The template for installing Cloudbreak will appear. On the **Basics** page, provide the following basic parameters:   

    **BASICS**

    | Parameter | Description |
|---|---|
| Subscription | Select which existing subscription you want to use. |
| Resource group | Select **Create new** if you would like to create a new resource group and enter a name for your new resource group. Alternatively, you can select an existing empty resource group. Cloudbreak resources will later be accessible in that chosen resource group. |
| Location | Select an Azure region in which you want to deploy Cloudbreak. |

    **SETTINGS**

    | Parameter | Description |
|---|---|
| VM Size | Select virtual machine instance type to use for the Cloudbreak controller. The minimum instance type suitable for Cloudbreak is *D2*. |
| Admin Username | Create an admin login that you will use to log in to the Cloudbreak UI. Must be a valid email address. |
| Admin User Password | Password for the admin login. Must be at least 8 characters containing letters, numbers, and symbols. |
| Username | Enter an admin username for the virtual machine. You will use it to SSH to the VM. |
| SmartSense | Select whether you want to use SmartSense telemetry. Default is "false" (not using SmartSense telemetry). |
| Remote Location | <p>Allow connections from this address range. Enter a valid [CIDR IP](http://www.ipaddressguide.com/cidr) or enter `INTERNET`. For example: </p><p><ul><li>10.0.0.0/24 will allow access from 10.0.0.0 through 10.0.0.255</li><li>INTERNET will allow access from all.</li><li>VIRTUAL_NETWORK and LOAD_BALANCER are currently not supported.</li></ul></p>  |
| Ssh Key| <p>Paste your SSH public key.</p><p>You can use `pbcopy` to quickly copy it. For example: `pbcopy < /Users/homedir/.ssh/id_rsa.pub`</p> |

4. Review terms of use and check "I agree to the terms and conditions stated above". 

5. Click **Purchase**.

6. Proceed to the next step: [Explore Newly Created Resources](#explore-newly-created-resources)


#### (Option 2) Launch from Azure Marketplace 

> This feature is Technical Preview.  

1. Log in to your [Azure Portal](https://portal.azure.com).

2. From the services menu, select <img src="../images/marketpl-icon.png" width="130" title="Icon">.

3. In the search box, enter "Cloudbreak":  

    <a href="../images/launch-marketpl-search.png" target="_blank" title="click to enlarge"><img src="../images/launch-marketpl-search.png" width="650" title="Azure Portal"></a>  

4. Select <img src="../images/launch-marketpl-contr.png" width="270" title="Icon">.  
    The information about the Cloudbreak for Hortonworks Data Platform will be displayed.
    In the bottom of the page, you will see a dropdown for selecting a deployment model, with the *Resource Manager* deployment model pre-selected. This is the only deployment model available for this offering.

5. Click **Create**.   

6. The template for installing Cloudbreak will appear. On the **Basics** page, provide the following basic parameters:

    | Parameter | Description |
|---|---|
| Administrator email address | Create an admin login that you will use to log in to the Cloudbreak UI. Must be a valid email address. |
| Administrator user password | Password for the admin login. Must be at least 8 characters containing letters, numbers, and symbols. |
| Confirm password | Confirm the password for the admin login. |
| VM Username | Enter an admin username for the virtual machine. You will use it to SSH to the VM. |
| VM SSH public key| <p>Paste your SSH public key.</p><p>You can use `pbcopy` to quickly copy it. For example: `pbcopy < /Users/homedir/.ssh/id_rsa.pub`</p> |
| Subscription | Select which existing subscription you want to use. |
| Resource group | Select **Create new** if you would like to create a new resource group and enter a name for your new resource group. Alternatively, you can select an existing empty resource group. Cloudbreak resources will later be accessible in that chosen resource group. |
| Location | Select an Azure region in which you want to deploy Cloudbreak. |

7. Once done, click **OK**.

8. On the **Advanced Settings** page, provide the following advanced parameters:

    | Parameter | Description |
|---|---|
| Controller Instance Type | Select virtual machine instance type to use for the Cloudbreak controller. The minimum instance type suitable for Cloudbreak is *D2*. For more information about instance types on Azure refer to [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general). |
| Allow connections to the cloud controller from this address range or [default tag](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg#default-tags) |<p>Enter a valid [CIDR IP](http://www.ipaddressguide.com/cidr) or enter `Internet`. For example: </p><p><ul><li>10.0.0.0/24 will allow access from 10.0.0.0 through 10.0.0.255</li><li>'Internet' will allow access from all.</li><li>(Advanced) 'VirtualNetwork' will allow access from the address space of the Virtual Network.</li><li> (Advanced) 'AzureLoadBalancer' will allow access from the address space of the load balancer.</li></ul></p><p>For more information, refer to the [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg#default-tags).</p> |
| Enable SmartSense | (Optional) Select whether to enable SmartSense telemetry. Default is "I have read and opt-in to SmartSense telemetry. For more information about SmartSense, refer to the [Hortonworks website](https://hortonworks.com/services/support/smartsense/). |
| Enter your existing SmartSenseID (if available). You must also opt-in to SmartSense telemetry above | (Optional) Enter your existing SmartSenseID. | 
| Virtual network | Create a new Vnet (default) or select an existing Vnet. |
| Subnets | If you created a new Vnet, create subnets within it. If selected an existing Vnet, select exiting subnets. |


9. Once done, click **OK**.

10. On the **Summary** page, validate the information that you provided. 
    Before proceeding to the next page, you have an option to **Download template and parameters**. You can use them to create the Cloudbreak controller via CLI. Once done, click **OK**.
    
11. Review terms of use and click **Purchase**.

12. Proceed to the next step: [Explore Newly Created Resources](#explore-newly-created-resources)


### Explore Newly Created Resources

> This step is optional.  

While the deployment is in progress, you can optionally navigate to the newly created resource group and see what Azure resources are being created.

1. From the left pane, select <img src="../images/resource-icon.png" width="150" title="Icon">.

2. Find the the resource group that you just created and select it to view details.

3. The following resources should have been created in your resource group:

    <a href="../images/azure-resources.png" target="_blank" title="click to enlarge"><img src="../images/azure-resources.png" width="650" title="Azure Portal"></a>
    
    > If you chose to use an existing virtual network, the virtual network will not be added to the resource group. 

    * [Virtual network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) (VNet) securely connects Azure resources to each other.    
    * [Network security group](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) (NSG) defines inbound and outbound security rules, which control network traffic flow.  
    * [Virtual machine](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-linux-azure-overview?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) runs Cloudbreak.   
    * [Public IP address](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm) is assigned to your VM so that it can communicate with other Azure resources.  
    * [Network interface](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface) (NIC) attached to the VM provides the interconnection between the VM and the underlying software network.    
    * [Blob storage container](https://docs.microsoft.com/en-us/azure/storage/storage-introduction) is created to store Cloudbreak Deployer OS disk's data.  

4. You can click on each entry to view details of the resource. For example, click on <img src="../images/cdeployer-icon.png" width="120" title="Icon"> to view details, including Cloudbreak IP address.

5. Once your deployment is ready, the status will change from "Deploying" to "Success".
    

### Access Cloudbreak UI

1. When your deployment succeeds, you will receive a notification in the top-right corner. Click on the notification to navigate to the deployed resource. 

3. From **Outputs**, you can copy the link by clicking on the <img src="../images/copy-icon.png" width="30" title="Icon"> icon:

    <a href="../images/cb-outputs.png" target="_blank" title="click to enlarge"><img src="../images/cb-outputs.png" width="650" title="Azure Portal"></a>   

2. Paste the link in your browser's address bar.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.

    | Browser | Steps |
|---|---|
| Firefox | Click **Advanced** > Click **Add Exception...** > Click **Confirm Security Exception** |
| Safari | Click **Continue** |
| Chrome | Click **Advanced** > Click **Proceed...** |  

3. Now you should be able to access Cloudbreak UI and log in with the **Admin email address** and **Admin password** that you created when launching Cloudbreak:

     <a href="../images/cb-ui.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui.png" width="650" title="Azure Portal"></a>

    The last task that you need to perform before you can use Cloudbreak is to [create a cloudbreak credential](#create-cloudbreak-credential).     


### Create Cloudbreak Credential


The first time you log in to the Cloudbreak UI, you should see **manage credentials** tab open, informing you that to use Cloudbreak you first need to create a credential associated with your Azure subscription. Cloudbreak works by connecting your Azure account through this credential, and then uses it to create resources on your behalf.

Before you can start provisioning cluster using Cloudbreak, you must create a Cloudbreak credential. There are two ways to do this: 

* **Interactive**: This is the recommended simpler method. The advantage of using this method is that the app and service principal creation and role assignment is fully automated, so the only input that you need to provide is the name and the SSH key. To configure an interactive credential, refer to [Create an Interactive Credential](#create-an-interactive-credential).  

* **App-based**: This is the more complex method. The advantage of the app-based credential creation is that it allows you to create a credential without logging in to the Azure account, as long as you have been given all the information. In addition to providing your `tenant_id` and `subscription_id`, you must provide information for your previously created Azure AD application, including its password. When using this method, we recommend that you use a utility called `azure-cli-tools`. The utility supports app creation and role assignment. It is available at [https://github.com/sequenceiq/azure-cli-tools/blob/master/cli_tools](https://github.com/sequenceiq/azure-cli-tools/blob/master/cli_tools). If you choose not to use this utility, the app-based configuration process is error-prone with many steps involved. To configure an app based credential, refer to [Create an App Based Credential](#create-an-app-based-credential).  


#### Create an Interactive Credential

1. Open the **manage credentials** pane:

     <a href="../images/cb-ui2.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui2.png" width="650" title="Azure Portal"></a>

2. Click **Next**.
3. On the **Configure credentials** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Select Credential Type | The **Interactive** credential type should be pre-selected. | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| SSH Public Key | The SSH key that you provided when launching Cloudbreak should be pre-entered. |
| Select Azure role type | <p>You have the following options:<ul><li>"Use existing Contributor role" (default): If you select this option, Cloudbreak will use the "[Contributor](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#contributor)" role to create resources. This requires no further input.</li><li>"Reuse existing custom role": If you select this option and enter the name of an existing role, Cloudbreak will use this role to create resources.</li><li>"Let Cloudbreak create a custom role": If you select this option and enter a name for the new role, the role will be created. When choosing role name, make sure that there is no existing role with the name chosen. For information on creating custom roles, refer to [Azure](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles) documentation. </li></ul></p><p>If using a custom role, make sure that it includes the necessary Action set for Cloudbreak to be able to manage clusters: `Microsoft.Compute/*`, `Microsoft.Network/*`, `Microsoft.Storage/*`, `Microsoft.Resources/*`.</p> |
| Select Platform | (Optional) Select a platform (if previously configured). |
| Public In Account | (Optional) If you check this, other users added to your Cloudbreak instance will be able to use this credential to create clusters. |
    

2. Click **Next**.
3. On the **Finish** page, click on the <img src="../images/copy-icon.png" width="35" title="Icon"> icon next to the randomly generated code to copy it.

4. Click **Azure login** and a new **Device login** page will open in a new browser tab:

     <a href="../images/device-login.png" target="_blank" title="click to enlarge"><img src="../images/device-login.png" width="650" title="Azure Portal"></a>  

5. Next, paste the code in field on the  **Device login** page and click **Continue**.
6. Confirm your account by selecting it:

     <a href="../images/device-login2.png" target="_blank" title="click to enlarge"><img src="../images/device-login2.png" width="650" title="Azure Portal"></a>

7. A confirmation page will appear, confirming that you have signed in to the Microsoft Azure Cross-platform Command Line Interface application on your device. You may now close this window.
8. Your credential should now be displayed in the **manage credentials** tab.

     Congratulations! You've successfully launched and configured Cloudbreak. Now you can use Cloudbreak to [create clusters](azure-create.md).
     
     
#### Create an App Based Credential

1. On Azure Portal, navigate to the **Azure Active Directory** and register a new application:

     <a href="../images/azure-appbased01.png" target="_blank" title="click to enlarge"><img src="../images/azure-appbased01.png" width="650" title="Azure Portal"></a>

1. Navigate to the **Subscriptions** and assign the "Contributor" role to your newly created application:

     <a href="../images/azure-appbased03.png" target="_blank" title="click to enlarge"><img src="../images/azure-appbased03.png" width="650" title="Azure Portal"></a>   

1. In the Cloudbreak web UI, open the **manage credentials** pane:

     <a href="../images/cb-ui2.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui2.png" width="650" title="Azure Portal"></a>

1. Click **Next**.

1. On the **Configure credential** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **App based**. | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| Subscription Id | Copy and paste the Subscription ID from your **Subscriptions**. |
| App Id | Copy and paste the Application ID from your **Azure Active Directory** app registration's **Settings** > **Properties**. |
| Password | This is your application key. You can obtain it from your from your **Azure Active Directory** app registration's **Settings** > **Keys**. |
| App Owner Tenant Id | Paste your App Owner Tenant Id. There are multiple ways to obtain it. For example, you can find it in the *PortalDiagnostics.json* file, which can be downloaded by clicking on <img src="../images/question-icon.png" width="30" title="Icon"> in the top-right corner and then selecting **Show Diagnostics**.  |
| SSH Public Key | The SSH key that you provided when launching Cloudbreak should be pre-entered. |
| Select Azure role type | <p>You have the following options:<ul><li>"Use existing Contributor role" (default): If you select this option, Cloudbreak will use the "[Contributor](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#contributor)" role to create resources. This requires no further input.</li><li>"Reuse existing custom role": If you select this option and enter the name of an existing role, Cloudbreak will use this role to create resources.</li><li>"Let Cloudbreak create a custom role": If you select this option and enter a name for the new role, the role will be created. When choosing role name, make sure that there is no existing role with the name chosen. For information on creating custom roles, refer to [Azure](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles) documentation. </li></ul></p><p>If using a custom role, make sure that it includes the necessary Action set for Cloudbreak to be able to manage clusters: `Microsoft.Compute/*`, `Microsoft.Network/*`, `Microsoft.Storage/*`, `Microsoft.Resources/*`.</p> |
| Select Platform | (Optional) Select a platform (if previously configured). |
| Public In Account | (Optional) If you check this, other users added to your Cloudbreak instance will be able to use this credential to create clusters. |

1. Click **Next** and then **Finish**.

1. Your credential should now be displayed at the top of the page in the **manage credentials** tab.

     Congratulations! You've successfully launched and configured Cloudbreak. 


<div class="next">
<a href="../azure-config/index.html">Next: Define Infrastructure Templates</a>
</div>

