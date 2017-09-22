## Launch Cloudbreak on AWS

Before launching Cloudbreak on AWS, review and meet the prerequisites. Next, launch a VM using a Cloudbreak Amazon Machine Image (AMI), SSH to the VM, and start Cloudbreak. Once Cloudbreak is running, log in to the Cloudbreak UI and create a Cloudbreak credential. 

### Meet the Prerequisites

Before launching Cloudbreak on AWS, you must meet the following prerequisites.

#### AWS Account 

In order to launch Cloudbreak on Azure, you must log in to your AWS account. If you don't have an account, you can create one at [https://aws.amazon.com/](https://aws.amazon.com/).

#### AWS Region

Decide in which AWS region you would like to launch Cloudbreak. The following AWS regions are supported: 

|Region Name | Region |
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

For detailed information about AWS regions, refer to <a href="http://docs.aws.amazon.com/general/latest/gr/rande.html" target="_blank">AWS documentation</a>. 

#### SSH Key Pair

Import an existing key pair or generate a new key pair in the AWS region which you are planning to use for launching Cloudbreak and clusters. You can access this option by navigating to the **EC2 Console** and selecting **NETWORK AND SECURITY** > **Key Pairs** from the left pane. Refer to [AWS documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) for detailed instructions on how to create a key pair in a selected region.

You will need this SSH key pair to SSH to the Cloudbreak instance and start Cloudbreak. 

#### Authentication 

Before you can start using Cloudbreak for provisioning clusters, you must create IAM roles that  Cloudbreak to connect to your AWS account and create resources on your behalf. There are two ways to do this: 

* **Key-based**: This is a simpler option which does not require additional configuration at this point. It requires that you provide your AWS access key and secret key pair in the Cloudbreak web UI later.

* **Role-based**: This requires that you or your AWS admin create two IAM roles: one to allow Cloudbreak to assume AWS roles ("AssumeRole" policy) and the second one to provide Cloudbreak with the capabilities required for cluster creation ("cb-policy" policy).

**Option 1: Key-based**

This option requires your AWS access key and secret key pair. Cloudbreak will use these keys to launch the resources. You must provide the access and secret keys later in the Cloudbreak web UI later when creating a credential. 

If you choose this option, all you need to do at this point is check your AWS account and make sure that you can access this key pair. You can generate new access and secret keys from the **IAM Console** > **Users**. Next, select a user and click on the **Security credentials** tab.
 

**Option 2: Role-based**

>>>> TO-DO: This section needs more work. We should add a video showing how to create a policy and then create a role. Also, the part about the cross-account access is not clear.

This requires that you create two IAM roles: one to grant Cloudbreak access to allow Cloudbreak to assume AWS roles (using the "AssumeRole" policy) and the second one to provide Cloudbreak with the capabilities required for cluster creation (using the "cb-policy" policy). You can perform these steps in the **IAM console**, on the **Policies** page via the **Create Policy** option, and on the **Roles** page via the **Create Role** option. 

1. Define a new policy called "AssumeRole" using the following policy definition and then create a role called "CloudbreakRole" and attach the "AssumeRole" policy to it. The "AssumeRole" policy definition is provided below.
    
2. Define another policy called "cb-policy" using the following policy definition and then create a new IAM role called "CredentialRole" and attach the "cb-policy" policy to it.  The "cb-policy" policy definition is provided below.

    When creating this role using the AWS Console, make sure that that it is a role for cross-account access and that the trust-relation is set up as follows: 'Account ID' is your own 12-digit AWS account ID and 'External ID' is “provision-ambari”.
    
    <a href="../images/aws-role.png" target="_blank" title="click to enlarge"><img src="../images/aws-role.png" width="650" title="IAM Console"></a> 

    > Alternatively, you can generate the "CredentialRole" role later once your VM is running and once you have [configured AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) on your VM by running the `cbd aws generate-role` command. This command creates a role with the name "cbreak-deployer" (equivalent to the "CredentialRole"). To customize the name of the role, add `export AWS_ROLE_NAME=my-cloudbreak-role-name` (where "my-cloudbreak-role-name" is your custom role name) as a new line to your Profile. 
    
This is all that you need to do at this point. Here is why you need these roles:     

|Role | Purpose | Configuration |
|----|---|---|
| CloudbreakRole | For Cloudbreak to assume other IAM roles - specifically the CredentialRole | <p>Option 1: When launching your Cloudbreak VM, during **Step 3: Configure Instance Details** > **IAM**, you will attach the "CloudbreakRole" IAM role to the VM.</p><p> Option 2: Alternatively, instead of attaching the "CloudbreakRole" role during the VM launch, you can assign the "CloudbreakRole" to an IAM user and then add the access and security key of that user to your 'Profile'.</p> |
| CredentialRole | For Cloudbreak to create AWS resources required for clusters | Once you log in to the Cloudbreak UI and are ready to create clusters, you will use this role to create the Cloudbreak Credential. | 


The "AssumeRole" policy definition: 

<pre>{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1400068149000",
      "Effect": "Allow",
      "Action": [
        "sts:AssumeRole"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}</pre>


The "cb-policy" policy definition: 

<pre>{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [ "cloudformation:*" ],
      "Resource": [ "*" ]
    },
    {
      "Effect": "Allow",
      "Action": [ "ec2:*" ],
      "Resource": [ "*" ]
    },
    {
      "Effect": "Allow",
      "Action": [ "iam:PassRole" ],
      "Resource": [ "*" ]
    },
    {
      "Effect": "Allow",
      "Action": [ "autoscaling:*" ],
      "Resource": [ "*" ]
    }
  ]
}</pre>

For more information about IAM, refer to <a href="http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html" target="_blank">Using Instance Profiles</a> and <a href="http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html" target="_blank">Using an IAM Role to Grant Permissions to Applications Running on Amazon EC2 Instances</a>.


### Launch the VM  

Now that you've met the prerequisites, you can launch the Cloudbreak deployer VM available as a Community AMI.

1. In the AWS Management Console, navigate to the EC2 Console.  

2. In the top right corner, select the region in which you want to launch Cloudbreak.  

    <a href="../images/aws-01.png" target="_blank" title="click to enlarge"><img src="../images/aws-01.png" width="650" title="EC2 Console"></a> 
    
3. From the left pane, select **INSTANCES** > **Instances**.  

4. Click on **Launch Instance**.

5. In **Step 1: Choose an Amazon Machine Image (AMI)** page, from the left pane, select **Community AMIs**. 

    <a href="../images/aws-02.png" target="_blank" title="click to enlarge"><img src="../images/aws-02.png" width="650" title="EC2 Console"></a> 
     
6. In the search box, enter the image name. The following Cloudbreak deplouer images are available:

    | Region | Image Name |
|---|---|
| eu-west-1 | ami-711d0317 |
| sa-east-1 |	 ami-bd365dd1 |
| us-east-1 |	 ami-10a78606 |
| us-west-1 |	 ami-01735e61 |
| us-west-2 |	 ami-6ccbc015 |
| eu-central-1 |	ami-d116b2be | 
| ap-northeast-1 | ami-97c4ccf0 |
| ap-southeast-1 | ami-d162e0b2 | 
| ap-southeast-2 | ami-a7dacbc4 |

    >>>>TO-DO: This table should be automatically generated.

7. Click **Select**.  

    > The steps listed below only mention required parameters. You may optionally review and adjust additional parameters. 
  
8. In **Step2: Choose Instance Type**, choose an instance type. The minimum instance type which is suitable for Cloudbreak is **m3.large**. Minimum requirements are 8GB RAM, 10GB disk, 2 cores. Click **Next**.

    <a href="../images/aws-03.png" target="_blank" title="click to enlarge"><img src="../images/aws-03.png" width="650" title="EC2 Console"></a>   
    
9. (Perform this step only if you are using role-based authorization) In **Step 3: Configure Instance Details** > **IAM**, select the "CloudbreakRole" IAM role which you [created earlier](#authorization-for-cloudbreak).

10. In **Step 6: Configure Security Group**, open the following ports: 22 (for access via SSH) and 443 (for access via HTTPS). Click **Review and Launch**.

    <a href="../images/aws-04.png" target="_blank" title="click to enlarge"><img src="../images/aws-04.png" width="650" title="EC2 Console"></a> 

12. In **Step 7: Review Instance Launch**, review the information carefully and then click **Launch**. 

13. When prompted select an existing key pair or create a new one. Next, acknowledge that you have access to the private key file and click **Launch Instance**. 

    <a href="../images/aws-05.png" target="_blank" title="click to enlarge"><img src="../images/aws-05.png" width="400" title="EC2 Console"></a>  
    
14. Click on the instance ID to navigate to the **Instances** view in your EC2 console. 

    <a href="../images/aws-06.png" target="_blank" title="click to enlarge"><img src="../images/aws-06.png" width="650" title="EC2 Console"></a>  


### SSH to the VM  

Now that your VM is ready, access it via SSH: 
  
* Use the private key from the key pair that you selected when launching the instance. 
* The SSH user is called "cloudbreak".
* You can obtain the host IP from the EC2 console > **Instances** view by selecting the instance, selecting the **Description** tab, and copying the value of the **Public DNS (IPv4)
** or **IPv4 Public IP** parameter.


### Launch Cloudbreak Deployer 

After accessing the VM via SSH: 

1. Navigate to the cloudbreak-deployment directory:

    <pre>cd /var/lib/cloudbreak-deployment/</pre>
    
    This directory contains configuration files and the supporting binaries for Cloudbreak deployer.
    
2.  Initialize your profile by creating a new file called `Profile` and adding the following content:

    <pre>export UAA_DEFAULT_SECRET=MY-SECRET
export UAA_DEFAULT_USER_PW=MY-PASSWORD</pre>  

    For example: 

    <pre>export UAA_DEFAULT_SECRET=MySecret123
export UAA_DEFAULT_USER_PW=MySecurePassword123</pre> 

    > You will need to provide the password when logging in to the Cloudbreak web UI and when using the Cloudbreak Shell. The secret will be used by Cloudbreak for authentication.  
    
3. Start the Cloudbreak application by using the following command:

    <pre>cbd start</pre>
    
    This will start the Docker containers and initialize the application. The first time you start the Coudbreak app, the process will take longer than usual due to the download of all the necessary docker images.
    
    The `cbd start` command includes the `cbd generate` command which applies the following steps:

    * Creates the `docker-compose.yml` file, which describes the configuration of all the Docker containers needed for the Cloudbreak deployment.
    * Creates the `uaa.yml` file, which holds the configuration of the identity server used to authenticate users with Cloudbreak.

    > Once the `cbd start` has finished, it returns the "Uluwatu (Cloudbreak UI) url" which you can later paste in your browser and log in to Cloudbreak web UI. 

5. Check Cloudbreak deployer version and health: 
    
    <pre>cbd doctor</pre>
    
5. Next, check Cloudbreak Application logs: 

    <pre>cbd logs cloudbreak</pre>
    
    You should see a message like this in the log: `Started CloudbreakApplication in 36.823 seconds.` Cloudbreak normally takes less than a minute to start.
    

### Access Cloudbreak UI

1. You can log into the Cloudbreak application at `https://IPv4_Public_IP>/` or `https://Public_DNS`. For example `https://34.212.141.253` or `https://ec2-34-212-141-253.us-west-2.compute.amazonaws.com`. 

2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.

3. Log in to the Cloudbreak web UI: 

    * The default username is `admin@example.com` but you should sign up with your own email address.
    * The password is the value of the `UAA_DEFAULT_USER_PW` variable that you configured in your `Profile` file when [launching Cloudbreak deployer](#launch-cloudbreak-deployer).

    <a href="../images/cloudbreak-ui.png" target="_blank" title="click to enlarge"><img src="../images/cloudbreak-ui.png" width="650" title="Cloudbreak web UI"></a>  

### Create Cloudbreak Credential


As part of the prerequisites, you had two options to [authorize Cloudbreak](#authorization-for-cloudbreak) to create resources on your behalf: key-based or role-based authorization. 

Depending on your earlier choice, you must configure a key-based or role-based credential. Without this credential, you will not be able to create clusters via Cloudbreak. 

#### Create Key-Based Credential

To perform these steps, you must know your access and secret key as well as your public SSH key. If needed, you can generate new access and secret keys from the **IAM Console** > **Users**. Next, select a user and click on the **Security credentials** tab. 
 
<a href="../images/aws-cred-key.png" target="_blank" title="click to enlarge"><img src="../images/aws-cred-key.png" width="650" title="Cloudbreak web UI"></a>  

1. In the Cloudbreak web UI, open the **manage credentials** pane. 

2. Click **+create credential**. 

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **Key Based**. | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| Access Key | Paste your access key. |
| Secret Access Key | Paste your secret key. |
| SSH Public Key | Paste your SSH public key. |
| Select Platform | (Optional) Select a platform (if previously configured). |
| Public In Account | (Optional) If you check this, other users added to your Cloudbreak instance will be able to use this credential to create clusters. |
 
4. Click **+create credential**.

5. Your credential should now be displayed in the **manage credentials** tab.

    Congratulations! You've successfully launched and configured Cloudbreak. Now you can use Cloudbreak to [create clusters](aws-create.md). 
 

#### Create Role-Based Credential

To perform these steps, you must know the **IAM Role ARN** corresponding to the "CredentialRole" (configured as a [prerequisite](#authorization-for-cloudbreak)). You must also have your public SSH key. 

<a href="../images/aws-cred-role.png" target="_blank" title="click to enlarge"><img src="../images/aws-cred-role.png" width="650" title="Cloudbreak web UI"></a>  

1. In the Cloudbreak web UI, open the **manage credentials** pane. 

2. Click **+create credential**. 

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Select Credential Type | Select **Role Based**. | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| IAM Role ARN | Paste the **IAM Role ARN** corresponding to the "CredentialRole" that you created earlier. For example `arn:aws:iam::315627065446:role/CredentialRole` |
| SSH Public Key | Paste your SSH public key. |
| Select Platform | (Optional) Select a platform (if previously configured). |
| Public In Account | (Optional) If you check this, other users added to your Cloudbreak instance will be able to use this credential to create clusters. |

4. Click **+create credential**.

5. Your credential should now be displayed in the **manage credentials** tab.

    Congratulations! You've successfully launched and configured Cloudbreak.
      

<div class="next">
<a href="../aws-create/index.html">Next: Create a Cluster</a>
</div>
