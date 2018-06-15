## Quickstart on Azure 

This quickstart documentation will help you get started with Cloudbreak. 

### Prerequisites 

In order to launch Cloudbreak from the ARM template you must:

* Have an existing an Azure account. If you don't have an account, you can create one at [https://azure.microsoft.com](https://azure.microsoft.com).

* Have an SSH key pair. If needed, you can generate a new SSH key pair:
    * On MacOS X and Linux using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`  
    * On Windows using [PuTTygen](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows)

* In order to create a Cloudbreak credential, you Azure account must have the minimum permissions described in [Azure roles](https://hortonworks.github.io/cloudbreak-documentation/latest/).    
    

### Launch Cloudbreak from the quickstart template 

Launch Cloudbreak from an Azure Resource Manager (ARM) template by using the following steps. This is the quickstart deployment option. 

**Steps**

1. Log in to your [Azure Portal](https://portal.azure.com).

2. Click here to get started with Cloudbreak installation using the Azure Resource Manager template:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhortonworks%2Fcbd-quickstart%2F2.7.0%2Fazure%2FmainTemplate.json">![deploy on azure](http://azuredeploy.net/deploybutton.png)</a>

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
| Vm Size | Select virtual machine instance type to use for the Cloudbreak controller. The minimum instance type suitable for Cloudbreak is *Standard_DS3*. The minimum requirements are 16GB RAM, 40GB disk, 4 cores. |
| Admin Username | Create an admin login that you will use to log in to the Cloudbreak UI. Must be a valid email address. By default, admin@example.com is used but you should change it to your email address. |
| Admin User Password | Password for the admin login. Must be at least 8 characters containing letters, numbers, and symbols. |
| Username | Enter an admin username for the virtual machine. You will use it to SSH to the VM. By default, "cloudbreak" is used. |
| Remote Location |<p>Enter a valid [CIDR IP](http://www.ipaddressguide.com/cidr) or use one of the default tags. Default value is `Internet` which allows access from all IP addresses. Examples: </p><p><ul><li>10.0.0.0/24 will allow access from 10.0.0.0 through 10.0.0.255</li><li>'Internet' will allow access from all. This is not a secure option but you can use it it you are just getting started and are not planning to have the instance on for a longer period. </li><li>(Advanced) 'VirtualNetwork' will allow access from the address space of the Virtual Network.</li><li> (Advanced) 'AzureLoadBalancer' will allow access from the address space of the load balancer.</li></ul></p><p>For more information, refer to the [Plan virtual networks](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) in Azure documentation.</p> |
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
  
    
### Access Cloudbreak web UI 

Follow these steps to obtain Cloudbreak VM's public IP address and log in to the Cloudbreak web UI. 

**Steps**       
    
 1. When your deployment succeeds, you will receive a notification in the top-right corner. You can click on the link provided to navigate to the resource group created earlier.

    > This only works right after deployment. At other times, you can find your resource group by selecting **Resource Groups** from the service menu and then finding your resource group by name.

2. Once you've navigated to your resource group, click on **Deployments** and then click on **Microsoft.Template**:

    <img src="../images/cb_azure-resource-find.png" width="650" title="Azure Portal">

3. From **Outputs**, you can copy the link by clicking on the copy icon:

    <img src="../images/cb_azure-outputs.png" width="650" title="Azure Portal">  

2. Paste the link in your browser's address bar.


2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.
        
    | Browser | 	Steps |
|---|---|
| Firefox |Click **Advanced** > Click **Add Exception...** > Click **Confirm Security Exception** |
| Safari | Click **Continue** |
| Chrome | Click **Advanced** > Click **Proceed...** |

3. The login page is displayed:

    <img src="../images/cb_cb-ui.png" width="650" title="Cloudbreak web UI">   

3. Now you should be able to access Cloudbreak UI and log in with the **Admin email address** and **Admin password** that you created when launching Cloudbreak. 

4. Upon a successful login, you are redirected to the dashboard:

    <img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI">  

The last task that you need to perform before you can use Cloudbreak is to create Cloudbreak credential.     


### Create interactive Cloudbreak credential 

Before you can start using Cloudbreak to create clusters, you must create a Cloudbreak credential. Cloudbreak credential allows Cloudbreak to authenticate with your Azure account and provision resources on your behalf.

There are two ways for Cloudbreak to authenticate with Azure: interactive and app-based. Since the interactive approach is simpler, the steps below describe how to configure an interactive Cloudbreak credential. 


Follow these steps to create an interactive Cloudbreak credential.

**Prerequisites**

Your account must have have an Owner role in order for the interactive credential creation to work.   

If your account does not have an Owner role, you must you the [app-based credential](https://hortonworks.github.io/cloudbreak-documentation/latest/) option instead of the interactive option. To review the requirements for both options. refer to [Azure roles](https://hortonworks.github.io/cloudbreak-documentation/latest/). 

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane.

2. Click **Create Credential**.

3. Under **Cloud provider**, select "Microsoft Azure".

4. Select **Interactive Login**:

    <img src="../images/cb_cb-azure-cred-inter.png" width="650" title="Cloudbreak web UI">     

5. Provide the following information:

    | Parameter | Description |
|---|---|
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| Subscription Id | Copy and paste the Subscription ID from your **Subscriptions**. |
| Tenant Id | Copy and paste your Directory ID from your **Active Directory** > **Properties**. |
| Azure role type | <p>You have the following options:<ul><li>"Use existing Contributor role" (default): If you select this option, Cloudbreak will use the "[Contributor](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles#contributor)" role to create resources. This requires no further input.</li><li>"Reuse existing custom role": If you select this option and enter the name of an existing role, Cloudbreak will use this role to create resources.</li><li>"Let Cloudbreak create a custom role": If you select this option and enter a name for the new role, the role will be created. When choosing role name, make sure that there is no existing role with the name chosen. For information on creating custom roles, refer to [Azure](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles) documentation. </li></ul></p><p>If using a custom role, make sure that it includes the necessary Action set for Cloudbreak to be able to manage clusters: `Microsoft.Compute/*`, `Microsoft.Network/*`, `Microsoft.Storage/*`, `Microsoft.Resources/*`.</p> |

    To obtain the **Subscription Id**:

     <img src="../images/cb_azure-cred-subscription.png" width="650" title="Cloudbreak web UI">   

    To obtain the **Tenant ID** (actually **Directory Id**):

    <img src="../images/cb_azure-cred-directoryid.png" width="650" title="Cloudbreak web UI">    

6. After providing the parameters, click **Interactive Login**.

6. Copy the code provided in the UI:

    <img src="../images/cb_cb-azure-cred-inter2.png" width="650" title="Cloudbreak web UI">     

7. Click **Azure login** and a new **Device login** page will open in a new browser tab:

     <img src="../images/cb_azure-device-login.png" width="650" title="Azure Portal">  

8. Next, paste the code in field on the  **Device login** page and click **Continue**.
9. Confirm your account by selecting it:

     <img src="../images/cb_azure-device-login2.png" width="650" title="Azure Portal">

10. A confirmation page will appear, confirming that you have signed in to the Microsoft Azure Cross-platform Command Line Interface application on your device. You may now close this window.

     Congratulations! You've successfully launched and configured Cloudbreak. Now you can use Cloudbreak to create clusters.


### Create a cluster     


Use these steps to create a cluster. This section only covers minimal steps required for creating a cluster based on the basic settings (2-node cluster with default hardware and storage options).  

**Steps**

1. Click the **Create Cluster** button and the *Create Cluster* wizard is displayed.  
    By default, **Basic** view is displayed.

    <img src="../images/cb_quick-create01.png" width="650" title="Cloudbreak web UI">  

3. On the **General Configuration** page:

    * **Cluster Name**: Enter a name for your cluster. The name must be between 5 and 40 characters, must start with a letter, and must only include lowercase letters, numbers, and hyphens.
    * **Region**: Select the region in which you would like to launch your cluster. 
    * **Cluster Type**: Choose one of default cluster configurations.  

4. Click **Next** three times to navigate to the **Security Page**. You do not need to enter anything on the **Hardware and Storage** and **Network** because by default Cloudbreak suggests the instance types, storage, and network to use (a new network and subnet is created by default).   

5. One the **Security** page, provide the following:

    * **Cluster User**: This will be the user that you should use to log in to Ambari and other cluster UIs. By default, this is `admin`.   
    * **Cluster Password**: Password for the cluster user.  
    * **SSH public key**: Select the existing public SSH key or paste your key. The key will be placed on the cluster VMs so that you can use the matching private key to access the VMs via SSH. 

    <img src="../images/cb_quick-create02.png" width="650" title="Cloudbreak web UI">  

6. Click on **Create Cluster** to create a cluster.

7. You will be redirected to the Cloudbreak dashboard, and a new tile representing your cluster will appear at the top of the page.

 
### Next steps

To learn how to access your cluster, refer to [Accessing a cluster](https://hortonworks.github.io/cloudbreak-documentation/latest/).

To learn how to use Cloudbreak to manage your cluster, refer to  [Managing and monitoring clusters](https://hortonworks.github.io/cloudbreak-documentation/latest/).


### Clean up after a failed deployment

#### Deleting clusters  

The proper way to delete clusters is to use the the **Terminate** option available in the Cloudbreak UI. If the terminate process fails, try the **Terminate** > **Force terminate** option.

If the force termination does not delete all cluster resources, delete the resources manually:

* To find the VMs, click on the links available in the cluster details. 
* To find the network and subnet, see the **Cluster Information** in the cluster details. 
* On Azure, you can delete the cluster manually by deleting the whole resource group created when the cluster was deployed. The name of the resource group, under which the cluster-related resources are organized always includes the name of the cluster, so you should be able to find the group by searching for that name in the **Resource groups**.

Upon cluster termination, Cloudbreak only terminates the resources that it created. It does not terminate any resources (such as networks, subnets, roles, and so on) which existed prior to cluster creation. 

#### Delete Cloudbreak on Azure

You can delete Cloudbreak instance from your Azure account by deleting related resources. To delete a Cloudbreak instance:

* If you deployed Cloudbreak in a new resource group: to delete Cloudbreak, delete the whole related resource group.

* If you deployed Cloudbreak in an existing resource group: navigate to the group and delete only Cloudbreak related resources such as the VM.


**Steps**

1. From the Microsoft Azure Portal dashboard, select **Resource groups**.

2. Find the resource group that you want to delete.

3. If you deployed Cloudbreak in a new resource group, you can delete the whole resource group. Click on **...** and select **Delete**:

    <img src="../images/cb_azure-delete.png" width="650" title="Azure Portal">  

    Next, type the name of the resource group to delete and click **Delete**.
    
4. If you deployed Cloudbreak in an existing resource group, navigate to the details of the resource group and delete only Cloudbreak-related resources such as the VM.    

