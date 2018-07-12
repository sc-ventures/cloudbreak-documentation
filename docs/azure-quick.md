## Quickstart on Azure 

This quickstart documentation will help you get started with Cloudbreak. 

### Prerequisites 

In order to launch Cloudbreak from the ARM template you must:

* Have an existing an Azure account. If you don't have an account, you can create one at [https://azure.microsoft.com](https://azure.microsoft.com).

* Have an SSH key pair. If needed, you can generate a new SSH key pair:
    * On MacOS X and Linux using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`  
    * On Windows using [PuTTygen](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows)

* In order to create a Cloudbreak credential, you Azure account must have the minimum permissions described in [Azure roles](azure-launch.md#azure-roles). 

**Related links**  
[Azure roles](azure-launch.md#azure-roles)
[PuTTygen](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) (External)      
    

### Launch Cloudbreak from the Quickstart Template 

Launch Cloudbreak from an Azure Resource Manager (ARM) template by using the following steps. This is the quickstart deployment option. 

**Steps**

1. Log in to your [Azure Portal](https://portal.azure.com).

2. Click here to get started with Cloudbreak installation using the Azure Resource Manager template:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhortonworks%2Fcbd-quickstart%2F2.4.3%2Fazure%2FmainTemplate.json">![deploy on azure](http://azuredeploy.net/deploybutton.png)</a>

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

    > If you encounter errors, refer to [Troubleshooting Cloudbreak on Azure](trouble-azure.md). 
    
    <div class="note">
    <p class="first admonition-title">Cleaning up after a failed deployment</p>
    <p class="last">For steps on how to delete Cloudbreak after a failed deployment, refer to <a href="../cb-delete/index.html#delete-cloudbreak-on-azure">Delete Cloudbreak on Azure</a>.</p>
</div>

**Related links**  
[Delete Cloudbreak on Azure](cb-delete.md#delete-cloudbreak-on-azure)  
[Troubleshooting Cloudbreak on Azure](trouble-azure.md)       
[CIDR IP](http://www.ipaddressguide.com/cidr)   
[Plan virtual networks](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) (External)    
    
### Access Cloudbreak Web UI 

Follow these steps to obtain Cloudbreak VM's public IP address and log in to the Cloudbreak web UI. 

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

4. Upon a successful login, you are redirected to the dashboard. 

The last task that you need to perform before you can use Cloudbreak is to create Cloudbreak credential.     

**Related links**  
[CIDR IP](http://www.ipaddressguide.com/cidr) (External)   
[Filter network traffic with network security groups](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) (External)  


### Create Cloudbreak Credential 

Before you can start using Cloudbreak to create clusters, you must create a Cloudbreak credential. Cloudbreak credential allows Cloudbreak to authenticate with your Azure account and provision resources on your behalf.

There are two ways for Cloudbreak to authenticate with Azure: interactive and app-based. Since the interactive approach is simpler, the steps below describe how to configure an interactive Cloudbreak credential. 


{!docs/common/azure-cred-int.md!}

### Create a Cluster     

{!docs/common/create-quick.md!}

### Next Steps

To learn how to access your cluster, refer to [Accessing a cluster](azure-clusters-access.md).

To learn how to use Cloudbreak to manage your cluster, refer to  [Managing and monitoring clusters](azure-clusters-manage.md).

