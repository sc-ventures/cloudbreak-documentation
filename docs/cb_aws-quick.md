## Quickstart on AWS

This quickstart documentation will help you get started with Cloudbreak. 

### Prerequisites   

In order to launch Cloudbreak from the CloudFormation template you must:

* Have an existing an AWS account. If you don't have an account, you can create one at [https://aws.amazon.com/](https://aws.amazon.com/).

* Import an existing SSH key pair or generate a new key pair in the AWS region which you are planning to use for launching Cloudbreak and clusters. If you don't have a key pair, refer to [SSH key pair](https://hortonworks.github.io/cloudbreak-documentation/latest/) documentation to create or import a key pair.  



### Launch Cloudbreak from the quickstart template  

Launch Cloudbreak from a CloudFormation template by using the following steps. This is the quickstart deployment option. 

**Steps** 

1. Click on the link to launch the CloudFormation template that will create the AWS resources, including an EC2 Instance running Cloudbreak:

 
    <table>
 <tr>
   <th>Region</th>
   <th>Link</th>
 </tr>
 <tr>
   <td>us-east-1 (N. Virginia)</td>
   <td><span class="button-instruction">
   <a href="https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US East</a></span></td>
 </tr>
  <tr>
   <td>us-west-1 (N. California)</td>
   <td><span class="button-instruction">
   <a href="https://us-west-1.console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US West (N. California)</a></span></td>
 </tr>
 <tr>
   <td>us-west-2 (Oregon)</td>
   <td><span class="button-instruction">
   <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US West (Oregon)</a></span></td>
 </tr>
 <tr>
   <td>eu-central-1 (Frankfurt)</td>
   <td><span class="button-instruction">
   <a href="https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in EU Central</a></span></td>
 </tr>
 <tr>
   <td>eu-west-1 (Dublin)</td>
   <td><span class="button-instruction">
   <a href="https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in EU West </a></span></td>
 </tr>
 <tr>
   <td>sa-east-1 (São Paulo)</td>
   <td><span class="button-instruction">
   <a href="https://sa-east-1.console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in South America</a></span></td>
 </tr>
 <tr>
   <td>ap-northeast-1 (Tokyo)</td>
   <td><span class="button-instruction">
   <a href="https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Tokyo)</a></span></td>
 </tr>
 <tr>
   <td>ap-southeast-1 (Singapore)</td>
   <td><span class="button-instruction">
   <a href="https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Singapore)</a></span></td>
 </tr>
 <tr>
   <td>ap-southeast-2 (Sydney)</td>
   <td><span class="button-instruction">
   <a href="https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Sydney)</a></span></td>
 </tr>
</table>  
     
               
1. The **Create stack** wizard is launched in the Amazon CloudFormation management console:  

1. You do not need to change any template parameters on the **Select Template** page, but check the region name in the top right corner to confirm the region in which you want to launch Cloudbreak:  

    <img src="../images/cb_aws-01.png" width="650" title="IAM Console"> 
    
    > If needed, you may change the region if needed by using the dropdown in the top right corner.

1. Click **Next** to display the **Specify Details** page.

1. On the the **Specify Details** page, enter the following information:

    <img src="../images/cb_aws-02.png" width="650" title="IAM Console"> 

    **Specify Details** section 

    | Parameter | Description |
    |---|---|
    | Stack name | Enter name for your stack. It must be unique in your AWS environment. |

    **General Configuration** 

    | Parameter | Description |
    |---|---|
    | Controller Instance Type | EC2 instance type to use for the cloud controller. |
    | Email Address | Username for the Admin login. Must be a valid email address. |
    | Admin Password | Password for Admin login. Must be at least 8 characters containing letters, numbers, and symbols. |
    
    **Security Configuration** 
        
    | Parameter | Description |    
    |---|---|
    | SSH Key Name | Name of an existing EC2 key pair to enable SSH to access the instances. Key pairs are region-specific, so only the key pairs that you created for a selected region will appear in the dropdown.If you don't have a key pair, refer to [SSH key pair](https://hortonworks.github.io/cloudbreak-documentation/latest/). |
    | Remote Access | Allow connections to the cloud controller ports from this address range. Must be a valid <a href="http://www.ipaddressguide.com/cidr" target="_blank">CIDR IP</a>. For example: <ul><li>192.168.27.0/24 will allow access from 192.168.27.0 through 192.168.27.255.</li><li>192.168.27.10/32 will allow access from 192.168.27.10.</li><li>0.0.0.0/0 will allow access from all.</li></ul> Refer to [Network security](https://hortonworks.github.io/cloudbreak-documentation/latest/) for more information on the inbound ports that are used with Cloudbreak. |

1. Click **Next**  to display the **Options** page.    

1. On he **Options** page, if you expand the **Advanced** section, there is an option to **Rollback on failure**. 
    * By
    default, this option is set to **Yes**, which means that if there are any event failures when creating
    the stack, all the AWS resources created so far are deleted (i.e rolled back) to avoid unnecessary charges. 
    * If you set this option to **No**, if there are any event failures when creating
    the stack, the
    resources are left intact (i.e. not rolled back). Select the **No** option to aid in
    troubleshooting. Note that in this case you are responsible for deleting the stack later.
    
[Comment]: <> (What about providing Permissions > IAM Role? Currently, it seems like CloudbreakRole is created by default. But this did not work well in HDCloud.)    
    
1. Click **Next** to display the **Review** page.
       
1. On the **Review** page, click the **I acknowledge...** checkbox.  

1. Click **Create**.

    > The **Stack Name** is shown in the table with a <span class="cfn-output">CREATE_IN_PROGRESS</span> status. You can click on the **Stack Name** and see the specific events that are in progress. The create process takes about 10 minutes and once ready, you will see <span class="cfn-output2">CREATE_COMPLETE</span>. 


### Access Cloudbreak web UI 

Follow these steps to obtain Cloudbreak VM's public IP address and log in to the Cloudbreak web UI. 

**Steps**     
    
1. Once the stack creation is complete, Cloudbreak is ready to use. You can obtain the URL to Cloudbreak from the **Outputs** tab:

    <img src="../images/cb_aws-05.png" width="650" title="CFN Console - Outputs">

    > If the Outputs tab is blank, refresh the page.

1. Paste the link in your browser's address bar.


2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.
        
    | Browser | 	Steps |
|---|---|
| Firefox |Click **Advanced** > Click **Add Exception...** > Click **Confirm Security Exception** |
| Safari | Click **Continue** |
| Chrome | Click **Advanced** > Click **Proceed...** |

3. The login page is displayed:

    <img src="../images/cb_cb-ui.png" width="650" title="Cloudbreak web UI">
    

1. Log in to the Cloudbreak web UI using the credential that you configured in the CloudFormation template.

1. Upon a successful login, you are redirected to the dashboard:

    <img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"> 
       
        
### Create key-based Cloudbreak credential

Before you can start using Cloudbreak to create clusters, you must create a Cloudbreak credential. Cloudbreak credential allows Cloudbreak to authenticate with your AWS account and provision resources on your behalf.


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

Upon cluster termination, Cloudbreak only terminates the resources that it created. It does not terminate any resources (such as networks, subnets, roles, and so on) which existed prior to cluster creation. 
 
#### Delete Cloudbreak on AWS

If you want to delete Cloudbreak deployment, you can do so by deleting the stack in the CloudFormation console.

**Steps**

1. Log in to the CloudFormation console.

1. Select the deployment that you want to delete.
 
1. select **Actions** > **Delete Stack**.

1. Click **Yes, Delete** to confirm.
 
 All resources created as part of this stack (such as the Cloudbreak VM) will be deleted. 
 