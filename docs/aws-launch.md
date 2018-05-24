## Launching Cloudbreak on AWS

Before launching Cloudbreak on AWS, review and meet the prerequisites. Next, launch a VM using a Cloudbreak Amazon Machine Image, access the VM, and then start Cloudbreak. Once Cloudbreak is running, log in to the Cloudbreak UI and create a Cloudbreak credential. 

### Meet the prerequisites

Before launching Cloudbreak on AWS, you must meet the following prerequisites.

#### AWS account 

In order to launch Cloudbreak on AWS, you must log in to your AWS account. If you don't have an account, you can create one at [https://aws.amazon.com/](https://aws.amazon.com/).

#### AWS region

Decide in which AWS region you would like to launch Cloudbreak. The following AWS regions are supported: 

| Region name | Region | 
|---|---|
| EU (Ireland) | eu-west-1 |
| EU (Frankfurt) | eu-central-1 |	
| US East (N. Virginia) | us-east-1 |
| US West (N. California) | us-west-1 |	
| US West (Oregon) | us-west-2 | 
| South America (São Paulo) | sa-east-1 | 
| Asia Pacific (Tokyo) | ap-northeast-1	|  
| Asia Pacific (Singapore) | ap-southeast-1 | 
| Asia Pacific (Sydney) | ap-southeast-2 | 

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it.

**Related links**  
[AWS regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) (External)   

#### SSH key pair

Import an existing key pair or generate a new key pair in the AWS region which you are planning to use for launching Cloudbreak and clusters. You can do this using the following steps.

**Steps** 

1. Navigate to the Amazon EC2 console at https://console.aws.amazon.com/ec2/.  
2. Check the region listed in the top right corner to make sure that you are in the correct region.  
3. In the left pane, find **NETWORK AND SECURITY** and click **Key Pairs**.   
4. Do one of the following:
    * Click **Create Key Pair** to create a new key pair. Your private key file will be automatically downloaded onto your computer. Make sure to save it in a secure location. You will need it to SSH to the cluster nodes. You may want to change access settings for the file using `chmod 400 my-key-pair.pem`.  
    * Click **Import Key Pair** to upload an existing public key and then select it and click **Import**. Make sure that you have access to its corresponding private key.    

You need this SSH key pair to SSH to the Cloudbreak instance and start Cloudbreak. 

**Related links**  
[Creating a key pair using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) (External)  

#### Authentication 

Before you can start using Cloudbreak for provisioning clusters, you must select a way for Cloudbreak to authenticate with your AWS account and create resources on your behalf. There are two ways to do this: 

* **Key-based**: This is a simpler option which does not require additional configuration at this point. It requires that you provide your AWS access key and secret key pair in the Cloudbreak web UI later. All you need to do now is check your AWS account and ensure that you can access this key pair.

* **Role-based**: This requires that you or your AWS admin create an IAM role to allow Cloudbreak to assume AWS roles (the "AssumeRole" policy).

### (Option 1) Configure key-based authentication 

If you are using key-based authentication for Cloudbreak on AWS, you must be able to provide your AWS access key and secret key pair. Cloudbreak will use these keys to launch the resources. You must provide the access and secret keys later in the Cloudbreak web UI later when creating a credential. 

If you choose this option, all you need to do at this point is check your AWS account and make sure that you can access this key pair. You can generate new access and secret keys from the **IAM Console** > **Users**. Next, select a user and click on the **Security credentials** tab:

<a href="../images/cb_aws-iam_security_creds.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws-iam_security_creds.png" width="650" title="IAM Console"></a> 

If you choose this option, you can proceed to [Launch Cloudbreak deployer from an image](#launch-cloudbreak-deployer-from-an-image).
 

### (Option 2) Configure role-based authentication

[Comment]: <> (TBD How IAM role will be selected when launching Cb from template.)

{!docs/common/aws-launch-authentication-role-based-intro.md!}

> These role and policy names are just examples. You may use different names when creating your resources.  

> CloudbreakRole: Alternatively, instead of attaching the "CloudbreakRole" role during the VM launch, you can assign the "CloudbreakRole" to an IAM user and then add the access and secret key of that user to your 'Profile'.

> CredentialRole: Alternatively you can generate the "CredentialRole" role later once your Cloudbreak VM is running by SSHing to the Cloudbreak VM and running the `cbd aws generate-role` command. This command creates a role with the name "cbreak-deployer" (equivalent to the "CredentialRole"). To customize the name of the role, add `export AWS_ROLE_NAME=my-cloudbreak-role-name` (where "my-cloudbreak-role-name" is your custom role name) as a new line to your Profile. If you choose this option, you must make sure that the "CloudbreakRole" or the IAM user have a permission not only to assume a role but also to create a role.  

You can create these roles in the **IAM console**, on the **Roles** page via the **Create Role** option. Detailed steps are provided below. 


#### Create CloudbreakRole

Use these steps to create CloudbreakRole. 

Use the following "AssumeRole" policy definition: 

<pre>{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "Stmt1400068149000",
    "Effect": "Allow",
    "Action": ["sts:AssumeRole"],
    "Resource": "*"
  }
}
</pre>  
 

**Steps**

1. Navigate to the **IAM console** > **Roles** and click **Create Role**.

    <a href="../images/cb_aws_role-00.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role-00.png" width="650" title="IAM Console"></a> 
    
2. In the "Create Role" wizard, select **AWS service** role type and then select any service. 

    <a href="../images/cb_aws_role-10.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role-10.png" width="650" title="IAM Console"></a> 

3. When done, click **Next: Permissions** to navigate to the next page in the wizard.
    
4. Click **Create policy**.

    <a href="../images/cb_aws_role-11.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role-11.png" width="650" title="IAM Console"></a>
    
5. Click **Select** next to "Create Your Own Policy".

    <a href="../images/cb_aws_role-12.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role-12.png" width="650" title="IAM Console"></a>  

6. In the **Policy Name** field, enter "AssumeRole" and in the **Policy Document** paste the policy definition. You can either copy it from the section preceding these steps or download and copy it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/AssumeRole.json).

    <a href="../images/cb_aws_role-13.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role-13.png" width="650" title="IAM Console"></a>  
    
7. When done, click **Create Policy**.

8. Click **Refresh**. Next, find the "AssumeRole" policy that you just created and select it by checking the box.

    <a href="../images/cb_aws_role-14.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role-14.png" width="650" title="IAM Console"></a> 
    
9. When done, click **Next: Review**.
    
9. In the **Roles name** field, enter role name, for example "CloudbreakRole". 

    <a href="../images/cb_aws_role-15.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role-15.png" width="650" title="IAM Console"></a> 
    
10. When done, click **Create role** to finish the role creation process.
    

#### Create CredentialRole

Use these steps to create CredentialRole.

Use the following "cb-policy" policy definition: 

<pre>{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeStackEvents",
        "cloudformation:DescribeStackResource",
        "cloudformation:DescribeStacks"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AllocateAddress",
        "ec2:AssociateAddress",
        "ec2:AssociateRouteTable",
        "ec2:AuthorizeSecurityGroupIngress",
        "ec2:DescribeRegions",
        "ec2:DescribeAvailabilityZones",
        "ec2:CreateRoute",
        "ec2:CreateRouteTable",
        "ec2:CreateSecurityGroup",
        "ec2:CreateSubnet",
        "ec2:CreateTags",
        "ec2:CreateVpc",
        "ec2:ModifyVpcAttribute",
        "ec2:DeleteSubnet",
        "ec2:CreateInternetGateway",
        "ec2:CreateKeyPair",
        "ec2:DeleteKeyPair", 
        "ec2:DisassociateAddress",
        "ec2:DisassociateRouteTable",
        "ec2:ModifySubnetAttribute",
        "ec2:ReleaseAddress",
        "ec2:DescribeAddresses",
        "ec2:DescribeImages",
        "ec2:DescribeInstanceStatus",
        "ec2:DescribeInstances",
        "ec2:DescribeInternetGateways",
        "ec2:DescribeKeyPairs",
        "ec2:DescribeRouteTables",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSubnets",
        "ec2:DescribeVpcs",
        "ec2:DescribeSpotInstanceRequests",
        "ec2:DescribeVpcAttribute",
        "ec2:ImportKeyPair",
        "ec2:AttachInternetGateway",
        "ec2:DeleteVpc",
        "ec2:DeleteSecurityGroup",
        "ec2:DeleteRouteTable",
        "ec2:DeleteInternetGateway",
        "ec2:DeleteRouteTable",
        "ec2:DeleteRoute",
        "ec2:DetachInternetGateway",
        "ec2:RunInstances",
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:ListRolePolicies",
        "iam:GetRolePolicy",
        "iam:ListAttachedRolePolicies",
        "iam:ListInstanceProfiles",
        "iam:PutRolePolicy",
        "iam:PassRole",
        "iam:GetRole"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:CreateAutoScalingGroup",
        "autoscaling:CreateLaunchConfiguration",
        "autoscaling:DeleteAutoScalingGroup",
        "autoscaling:DeleteLaunchConfiguration",
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeLaunchConfigurations",
        "autoscaling:DescribeScalingActivities",
        "autoscaling:DetachInstances",
        "autoscaling:ResumeProcesses",
        "autoscaling:SuspendProcesses",
        "autoscaling:UpdateAutoScalingGroup"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}</pre>


**Steps**

1. Navigate to the **IAM console** > **Roles** and click **Create Role**.

    <a href="../images/cb_aws_role0.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role0.png" width="650" title="IAM Console"></a> 
    
2. In the "Create Role" wizard, select **Another AWS account** role type. Next, provide the following:

    * In the **Account ID** field, enter your AWS account ID.
    * Under **Options**, check **Require external ID**.
    * In the **External ID**, enter "provision-ambari".

    <a href="../images/cb_aws_role1.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role1.png" width="650" title="IAM Console"></a> 
    
3. When done, click **Next: Permissions** to navigate to the next page in the wizard.
    
4. Click **Create policy**.

    <a href="../images/cb_aws_role2.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role2.png" width="650" title="IAM Console"></a>
    
5. Click **Select** next to "Create Your Own Policy".

    <a href="../images/cb_aws_role3.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role3.png" width="650" title="IAM Console"></a> 
    
    
6. In the **Policy Name** field, enter "cb-policy" and in the **Policy Document** paste the policy definition.  You can either copy it from the section preceding these steps or download and copy it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/cb-policy.json).

    <a href="../images/cb_aws_role4.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role4.png" width="650" title="IAM Console"></a>  
    
7. When done, click **Create Policy**.

8. Click **Refresh**. Next, find the "cb-policy" that you just created and select it by checking the box.

    <a href="../images/cb_aws_role5.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role5.png" width="650" title="IAM Console"></a> 
    
9. When done, click **Next: Review**.
    
10. In the **Roles name** field, enter role name, for example "CredentialRole". 

    <a href="../images/cb_aws_role6.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws_role6.png" width="650" title="IAM Console"></a> 
    
11. When done, click **Create role** to finish the role creation process. 

Once you are done, you can proceed to launch Cloudbreak.  

**Related links**  
[Using instance profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) (External)  
[Using an IAM role to grant permissions to applications](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) (External)   


### Launch Cloudbreak from a template  

Follow these steps to launch Cloudbreak by using an Amazon CloudFormation template:    

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


### Access Cloudbreak web UI

1. Once the stack creation is complete, the cloud controller is ready to use. You can obtain the URL to the cloud controller
and the SSH access information from the **Outputs** tab:

    <a href="../images/cb_aws-05.png" target="_blank" title="click to enlarge"><img src="../images/cb_aws-05.png" width="650" title="CFN Console - Outputs"></a>

    > If the Outputs tab is blank, refresh the page.

1. Once the stack creation is complete, browse instance created at the **CloudURL** provided in the **Outputs** tab and log in.

1. Log in to the Cloudbreak web UI using the credential that you configured.

1. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a>  


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

