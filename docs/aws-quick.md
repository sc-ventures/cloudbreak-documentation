## Quickstart on AWS

This quickstart documentation will help you get started with Cloudbreak. 

### Prerequisites   

In order to launch Cloudbreak from the CloudFormation template you must:

* Have an existing an AWS account. If you don't have an account, you can create one at [https://aws.amazon.com/](https://aws.amazon.com/).

* Import an existing SSH key pair or generate a new key pair in the AWS region which you are planning to use for launching Cloudbreak and clusters. If you don't have a key pair, refer to [SSH key pair](aws-launch.md#ssh-key-pair) documentation to create or import a key pair.  

**Related links**    
[SSH key pair](aws-launch.md#ssh-key-pair)   


### Launch Cloudbreak from the Quickstart Template  

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
   <a href="https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US East</a></span></td>
 </tr>
  <tr>
   <td>us-west-1 (N. California)</td>
   <td><span class="button-instruction">
   <a href="https://us-west-1.console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US West (N. California)</a></span></td>
 </tr>
 <tr>
   <td>us-west-2 (Oregon)</td>
   <td><span class="button-instruction">
   <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in US West (Oregon)</a></span></td>
 </tr>
 <tr>
   <td>eu-central-1 (Frankfurt)</td>
   <td><span class="button-instruction">
   <a href="https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in EU Central</a></span></td>
 </tr>
 <tr>
   <td>eu-west-1 (Dublin)</td>
   <td><span class="button-instruction">
   <a href="https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in EU West </a></span></td>
 </tr>
 <tr>
   <td>sa-east-1 (São Paulo)</td>
   <td><span class="button-instruction">
   <a href="https://sa-east-1.console.aws.amazon.com/cloudformation/home?region=sa-east-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in South America</a></span></td>
 </tr>
 <tr>
   <td>ap-northeast-1 (Tokyo)</td>
   <td><span class="button-instruction">
   <a href="https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Tokyo)</a></span></td>
 </tr>
 <tr>
   <td>ap-southeast-1 (Singapore)</td>
   <td><span class="button-instruction">
   <a href="https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Singapore)</a></span></td>
 </tr>
 <tr>
   <td>ap-southeast-2 (Sydney)</td>
   <td><span class="button-instruction">
   <a href="https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?templateURL=https://s3.amazonaws.com/cbd-quickstart/cbd-quickstart-2.4.3.template" target="_blank">
   <i class="fa fa-rocket" aria-hidden="true"></i> Launch the CloudFormation Template in Asia Pacific (Sydney)</a></span></td>
 </tr>
</table>  
     
               
1. The **Create stack** wizard is launched in the Amazon CloudFormation management console:  

1. You do not need to change any template parameters on the **Select Template** page, but check the region name in the top right corner to confirm the region in which you want to launch Cloudbreak:  

    <a href="../images/cb_aws-01.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws-01.png" width="650" title="IAM Console"></a>  
    
    > If needed, you may change the region if needed by using the dropdown in the top right corner.

1. Click **Next** to display the **Specify Details** page.

1. On the the **Specify Details** page, enter the following information:

    <a href="../images/cb_aws-02.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws-02.png" width="650" title="IAM Console"></a> 

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
    | SSH Key Name | Name of an existing EC2 key pair to enable SSH to access the instances. Key pairs are region-specific, so only the key pairs that you created for a selected region will appear in the dropdown.If you don't have a key pair, refer to [SSH key pair](aws-launch.md#ssh-key-pair). |
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
    
    <div class="note">
    <p class="first admonition-title">Cleaning up after a failed deployment</p>
    <p class="last">For steps on how to delete Cloudbreak after a failed deployment, refer to <a href="../cb-delete/index.html#delete-cloudbreak-on-aws">Delete Cloudbreak on AWS</a>.</p>
</div>

**Related links**  
[Delete Cloudbreak on AWS](cb-delete.md#delete-cloudbreak-on-aws)  
[Network security](security.md#network-security)  
[SSH key pair](aws-launch.md#ssh-key-pair)  
[CIDR IP](http://www.ipaddressguide.com/cidr) (External)  

### Access Cloudbreak Web UI 

Follow these steps to obtain Cloudbreak VM's public IP address and log in to the Cloudbreak web UI. 

**Steps**     
    
1. Once the stack creation is complete, Cloudbreak is ready to use. You can obtain the URL to Cloudbreak from the **Outputs** tab:

    <a href="../images/cb_aws-05.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws-05.png" width="650" title="CFN Console - Outputs"></a>

    > If the Outputs tab is blank, refresh the page.

1. Paste the link in your browser's address bar.

{!docs/common/launch-access-ui.md!} 

1. Log in to the Cloudbreak web UI using the credential that you configured in the CloudFormation template.

1. Upon a successful login, you are redirected to the dashboard.
       
        
### Create Cloudbreak Credential

Before you can start using Cloudbreak to create clusters, you must create a Cloudbreak credential. Cloudbreak credential allows Cloudbreak to authenticate with your AWS account and provision resources on your behalf.


{!docs/common/aws-cred-key.md!}

### Create a Cluster     

{!docs/common/create-quick.md!}


### Next Steps

To learn how to access your cluster, refer to [Accessing a cluster](aws-clusters-access.md).

To learn how to use Cloudbreak to manage your cluster, refer to  [Managing and monitoring clusters](aws-clusters-manage.md).

 