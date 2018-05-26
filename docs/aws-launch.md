## Launching Cloudbreak on AWS

Before launching Cloudbreak on AWS:

* Review and meet the [prerequisites](aws-pre.md).  
* If you haven't already, review the [deployment options](deployment-options.md).  

The steps below describe how to launch Cloudbreak by using one of the two available deployment options (quickstart and production), access Cloudbreak web UI, and create a Cloudbreak credential. 

### (Quickstart) Launch Cloudbreak from a template  

Follow these steps to launch Cloudbreak by using an Amazon CloudFormation template.  

> This is the AWS quickstart option. If you would like to install Cloudbreak on your own VM, refer to [Launch Cloudbreak on your own VM](vm-launch.md). 

**Steps** 

[Comment]: <> (TBD if custom VPC can be included.)
[Comment]: <> (TBD how roles will be selected.)

1. Click on the link to launch the CloudFormation template that will create the AWS resources, including an EC2 Instance running Cloudbreak:

 
    <table>
 <tr>
   <th>Region</th>
   <th>Link</th>
 </tr>
 <tr>
   <td>us-east-1 (N. Virginia)</td>
   <td><span class="button-instruction">
   <a href="https://us-east-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US East</a></span></td>
 </tr>
  <tr>
   <td>us-west-1 (N. California)</td>
   <td><span class="button-instruction">
   <a href="https://us-west-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US West (N. California)</a></span></td>
 </tr>
 <tr>
   <td>us-west-2 (Oregon)</td>
   <td><span class="button-instruction">
   <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US West (Oregon)</a></span></td>
 </tr>
 <tr>
   <td>eu-central-1 (Frankfurt)</td>
   <td><span class="button-instruction">
   <a href="https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in EU Central</a></span></td>
 </tr>
 <tr>
   <td>eu-west-1 (Dublin)</td>
   <td><span class="button-instruction">
   <a href="https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in EU West </a></span></td>
 </tr>
 <tr>
   <td>sa-east-1 (São Paulo)</td>
   <td><span class="button-instruction">
   <a href="https://sa-east-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in South America</a></span></td>
 </tr>
 <tr>
   <td>ap-northeast-1 Asia Pacific (Tokyo)</td>
   <td><span class="button-instruction">
   <a href="https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Tokyo)</a></span></td>
 </tr>
 <tr>
   <td>ap-southeast-1 (Singapore)</td>
   <td><span class="button-instruction">
   <a href="https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.7.0-dev.170.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Singapore)</a></span></td>
 </tr>
 <tr>
   <td>ap-southeast-2 (Sydney)</td>
   <td><span class="button-instruction">
   <a href="LINK" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Sydney)</a></span></td>
 </tr>
</table>  
     
               
1. The **Create stack** wizard is launched in the Amazon CloudFormation management console:  

1. In the top right corner, confirm the region in which you want to launch Cloudbreak:  

    <a href="/images/cb_aws-01.png" target="_blank" title="click to enlarge"><img src="/images/cb_aws-01.png" width="650" title="IAM Console"></a>  
    
    > You may change the region if needed by using the dropdown in the top right corner.

1. You do not need to change any template parameters on the **Select Template** page. Click **Next** to display the **Specify Details** page:

    <a href="/images/cb_aws-02.png" target="_blank" title="click to enlarge"><img src="/images/cb_aws-02.png" width="650" title="IAM Console"></a> 

1. On the the **Specify Details** page, enter the **Stack name**:

    | Parameter | Description |
    |---|---|
    | Stack name | Enter name for your stack. It must be unique in your AWS environment. |

1. On the same page, enter the following in the **Parameters** section:

    > All parameters are required.

    **General Configuration** 

    | Parameter | Description |
    |---|---|
    | Controller Instance Type | EC2 instance type to use for the cloud controller. |
    | Email Address | Username for the Admin login. Must be a valid email address. |
    | Admin Password | Password for Admin login. Must be at least 8 characters containing letters, numbers, and symbols. |
    
    **Security Configuration** 
        
    | Parameter | Description |    
    |---|---|
    | SSH Key Name | Name of an existing EC2 key pair to enable SSH to access the instances. Key pairs are region-specific, so only the key pairs that you created for a selected region will appear in the dropdown. See [Prerequisites](#prerequisites) for more information. |
    | Remote Access | Allow connections to the cloud controller ports from this address range. Must be a valid <a href="http://www.ipaddressguide.com/cidr" target="_blank">CIDR IP</a>. For example: <ul><li>192.168.27.0/24 will allow access from 192.168.27.0 through 192.168.27.255.</li><li>192.168.27.10/32 will allow access from 192.168.27.10.</li><li>0.0.0.0/0 will allow access from all.</li></ul> Refer to [Network security](security.md#network-security) for more information on the inbound ports that are used with Cloudbreak. |

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
    
    
1. Once the stack creation is complete, Cloudbreak is ready to use. You can obtain the URL to the cloud controller
and the SSH access information from the **Outputs** tab:

    <a href="../images/cb_aws-05.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws-05.png" width="650" title="CFN Console - Outputs"></a>

    > If the Outputs tab is blank, refresh the page.

1. Once the stack creation is complete, browse instance created at the **CloudURL** provided in the **Outputs** tab and log in.

1. Log in to the Cloudbreak web UI using the credential that you configured.

1. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a>  
        
    
### (Production) Launch Cloudbreak on your own VM

Refer to [Launch Cloudbreak on your own VM](vm-launch.md). 


### Create Cloudbreak credential

Before you can start creating clusters, you must first create a [Cloudbreak credential](architecture.md#cloudbreak-credential). Without this credential, you will not be able to create clusters via Cloudbreak. 

As part of the [prerequisites](#authentication), you had two options to allow Cloudbreak to authenticate with AWS and create resources on your behalf: key-based or role-based authentication. Depending on your choice, you must configure a key-based or role-based credential: 

* [Create key-based credential](#create-key-based-credential)  
* [Create role-based credential](#create-role-based-credential)


#### Create key-based credential

To perform these steps, you must know your access and secret key. If needed, you or your AWS administrator can generate new access and secret keys from the **IAM Console** > **Users** > select a user > **Security credentials**. 

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
 

#### Create role-based credential

To perform these steps, you must know the **IAM Role ARN** corresponding to the "CredentialRole" (configured as a [prerequisite](#authentication)).  

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

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloubdreak to [create clusters](aws-create.md). 
      

<div class="next">
<a href="../aws-create/index.html">Next: Create a Cluster</a>
</div>

