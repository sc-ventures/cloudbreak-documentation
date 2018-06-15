## Prerequisites on AWS

Before launching Cloudbreak on AWS, you must meet the following prerequisites.

### AWS account

In order to launch Cloudbreak on AWS, you must log in to your AWS account. If you don't have an account, you can create one at [https://aws.amazon.com/](https://aws.amazon.com/).

### AWS region

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
  

### SSH key pair

Import an existing key pair or generate a new key pair in the AWS region which you are planning to use for launching Cloudbreak and clusters. You can do this using the following steps.

**Steps**

1. Navigate to the Amazon EC2 console at https://console.aws.amazon.com/ec2/.  
2. Check the region listed in the top right corner to make sure that you are in the correct region.  
3. In the left pane, find **NETWORK AND SECURITY** and click **Key Pairs**.   
4. Do one of the following:
    * Click **Create Key Pair** to create a new key pair. Your private key file will be automatically downloaded onto your computer. Make sure to save it in a secure location. You will need it to SSH to the cluster nodes. You may want to change access settings for the file using `chmod 400 my-key-pair.pem`.  
    * Click **Import Key Pair** to upload an existing public key and then select it and click **Import**. Make sure that you have access to its corresponding private key.    

You need this SSH key pair to SSH to the Cloudbreak instance and start Cloudbreak.


### Virtual network

You must have a virtual network configured on your cloud provider.

### Security group

Ports 22 (SSH), 80 (HTTPS), and 443 (HTTPS) must be open on the security group.


### Authentication

Before you can start using Cloudbreak for provisioning clusters, you must select a way for Cloudbreak to authenticate with your AWS account and create resources on your behalf. There are two ways to do this:

* **Key-based**: This is a simpler option which does not require additional configuration at this point. It requires that you provide your AWS access key and secret key pair in the Cloudbreak web UI later. All you need to do now is check your AWS account and ensure that you can access this key pair.

* **Role-based**: This requires that you or your AWS admin create an IAM role to allow Cloudbreak to assume AWS roles (the "AssumeRole" policy).


### Use key-based authentication

If you are using key-based authentication for Cloudbreak on AWS, you must be able to provide your AWS access key and secret key pair. Cloudbreak will use these keys to launch the resources. You must provide the access and secret keys later in the Cloudbreak web UI later when creating a credential.

If you choose this option, all you need to do at this point is check your AWS account and make sure that you can access this key pair. You can generate new access and secret keys from the **IAM Console** > **Users**. Next, select a user and click on the **Security credentials** tab:

<img src="../images/cb_aws-iam_security_creds.png" width="650" title="IAM Console">

The minimum set of permissions required by Cloudbreak are the same as for the [CredentialRole](https://hortonworks.github.io/cloudbreak-documentation/latest/). 

If you choose this option, you can proceed to launch Cloudbreak.


### Configure role-based authentication

[Comment]: <> (TBD How IAM role will be selected when launching Cb from template.)

If you are using role-based authentication for Cloudbreak on AWS, you must create two IAM roles: one to grant Cloudbreak access to allow Cloudbreak to assume AWS roles (using the "AssumeRole" policy) and the second one to provide Cloudbreak with the capabilities required for cluster creation (using the "cb-policy" policy).

The following table provides contextual information about the two roles required: 

|Role | Purpose | Overview of steps | Configuration |
|----|---|---|---|
| **CloudbreakRole** | Allows Cloudbreak to assume other IAM roles - specifically the CredentialRole. | Create a role called "CloudbreakRole" and attach the "AssumeRole" policy. The "AssumeRole" policy definition and steps for creating the CloudbreakRole are provided below. | <p>When launching Cloudbreak, you will attach the "CloudbreakRole" IAM role to the VM.</p><p>If you are using hosted Cloudbreak, you do not need to perform this step.</p> |
| **CredentialRole** | Allows Cloudbreak to create AWS resources required for clusters. | <p>Create a new IAM role called "CredentialRole" and attach the "cb-policy" policy to it. The "cb-policy" policy definition and steps for creating the CredentialRole are provided below.</p><p> When creating this role using the AWS Console, make sure that that it is a role for cross-account access and that the trust-relation is set up as follows: 'Account ID' is your own 12-digit AWS account ID and 'External ID' is “provision-ambari”. See steps below.</p> | Once you log in to the Cloudbreak UI and are ready to create clusters, you will use this role to create the Cloudbreak credential. | 

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

    <img src="../images/cb_aws_role-00.png" width="650" title="IAM Console">

2. In the "Create Role" wizard, select **AWS service** role type and then select any service.

    <img src="../images/cb_aws_role-10.png" width="650" title="IAM Console">

3. When done, click **Next: Permissions** to navigate to the next page in the wizard.

4. Click **Create policy**.

    <img src="../images/cb_aws_role-11.png" width="650" title="IAM Console">

5. Click **Select** next to "Create Your Own Policy".

    <img src="../images/cb_aws_role-12.png" width="650" title="IAM Console">  

6. In the **Policy Name** field, enter "AssumeRole" and in the **Policy Document** paste the policy definition. You can either copy it from the section preceding these steps or download and copy it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/AssumeRole.json).

    <img src="../images/cb_aws_role-13.png" width="650" title="IAM Console">  

7. When done, click **Create Policy**.

8. Click **Refresh**. Next, find the "AssumeRole" policy that you just created and select it by checking the box.

    <img src="../images/cb_aws_role-14.png" width="650" title="IAM Console">

9. When done, click **Next: Review**.

9. In the **Roles name** field, enter role name, for example "CloudbreakRole".

    <img src="../images/cb_aws_role-15.png" width="650" title="IAM Console">

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

    <img src="../images/cb_aws_role0.png" width="650" title="IAM Console">

2. In the "Create Role" wizard, select **Another AWS account** role type. Next, provide the following:

    * In the **Account ID** field, enter your AWS account ID.
    * Under **Options**, check **Require external ID**.
    * In the **External ID**, enter "provision-ambari".

    <img src="../images/cb_aws_role1.png" width="650" title="IAM Console">

3. When done, click **Next: Permissions** to navigate to the next page in the wizard.

4. Click **Create policy**.

    <img src="../images/cb_aws_role2.png" width="650" title="IAM Console">

5. Click **Select** next to "Create Your Own Policy".

    <img src="../images/cb_aws_role3.png" width="650" title="IAM Console">


6. In the **Policy Name** field, enter "cb-policy" and in the **Policy Document** paste the policy definition.  You can either copy it from the section preceding these steps or download and copy it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/cb-policy.json).

    <img src="../images/cb_aws_role4.png" width="650" title="IAM Console">  

7. When done, click **Create Policy**.

8. Click **Refresh**. Next, find the "cb-policy" that you just created and select it by checking the box.

    <img src="../images/cb_aws_role5.png" width="650" title="IAM Console">

9. When done, click **Next: Review**.

10. In the **Roles name** field, enter role name, for example "CredentialRole".

    <img src="../images/cb_aws_role6.png" width="650" title="IAM Console">

11. When done, click **Create role** to finish the role creation process.

Once you are done, you can proceed to launch Cloudbreak.  


## Launching Cloudbreak on AWS

These steps describe how to launch Cloudbreak on AWS for production. 
Before launching Cloudbreak on AWS, review and meet the AWS-specific [prerequisites](https://hortonworks.github.io/cloudbreak-documentation/latest/). Next, follow the steps below.  
        
    

### VM requirements

To launch the Cloudbreak deployer and install the Cloudbreak application, you must have an existing VM. 

#### System requirements  

Your system must meet the following requirements:

* Minimum VM requirements: 16GB RAM, 40GB disk, 4 cores
* Supported operating systems: RHEL, CentOS, and Oracle Linux 7 (64-bit)

> You can install Cloudbreak on Mac OS X for evaluation purposes only. Mac OS X is not supported for a production deployment of Cloudbreak.


#### Root access

Every command must be executed as root. In order to get root privileges execute:

<pre>sudo -i</pre>


#### System updates

Ensure that your system is up-to-date by executing:

<pre>yum -y update</pre>

Reboot it if necessary.


#### Install iptables

Perform these steps to install and configure iptables.

**Steps** 

1. Install iptables-services:

    <pre>yum -y install net-tools ntp wget lsof unzip tar iptables-services
systemctl enable ntpd && systemctl start ntpd
systemctl disable firewalld && systemctl stop firewalld</pre>

    > Without iptables-services installed the `iptables save` command will not be available.

2. Configure permissive iptables on your machine:

    <pre>iptables --flush INPUT && \
iptables --flush FORWARD && \
service iptables save</pre>


#### Disable SELINUX

Perform these steps to disable SELINUX.

**Steps** 

1. Disable SELINUX:
    
    <pre>setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' 
/etc/selinux/config</pre>

2. Run the following command to ensure that SELinux is not turned on afterwards: 

    <pre>getenforce</pre>
    
3. The command should return "Disabled".     

    
#### Install Docker 

Perform these steps to install Docker. The minimum Docker version is 1.13.1. If you are using an older image that comes with an older Docker version, upgrade Docker to 1.13.1 or newer. 

**Steps**    

1. Install Docker service:

    <pre>yum install -y docker
systemctl start docker
systemctl enable docker</pre>

3. Check the Docker Logging Driver configuration:

    <pre>docker info | grep "Logging Driver"</pre>
    
4. If it is set to `Logging Driver: journald`, you must  set it to "json-file" instead. To do that:

    1. Open the `docker` file for editing:
    
        <pre>vi /etc/sysconfig/docker</pre>  
        
    2. Edit the following part of the file so that it looks like below (showing `log-driver=json-file`):

        <pre># Modify these options if you want to change the way the docker daemon runs
OPTIONS='--selinux-enabled --log-driver=json-file --signature-verification=false'</pre>     

    3. Restart Docker:

        <pre>systemctl restart docker
systemctl status docker</pre>



### Install Cloudbreak on a VM

Install Cloudbreak using the following steps.


**Steps**

1. Install the Cloudbreak deployer and unzip the platform-specific single binary to your PATH. For example:

    <pre>yum -y install unzip tar
curl -Ls public-repo-1.hortonworks.com/HDP/cloudbreak/cloudbreak-deployer_2.7.0_$(uname)_x86_64.tgz | sudo tar -xz -C /bin cbd
cbd --version</pre>


    Once the Cloudbreak deployer is installed, you can set up the Cloudbreak application.

2. Create a Cloudbreak deployment directory and navigate to it:

    <pre>mkdir cloudbreak-deployment
cd cloudbreak-deployment</pre>

3. In the directory, create a file called `Profile` with the following content:

    <pre>export UAA_DEFAULT_SECRET=MY-SECRET
export UAA_DEFAULT_USER_PW=MY-PASSWORD
export UAA_DEFAULT_USER_EMAIL=MY-EMAIL
export PUBLIC_IP=MY_VM_IP</pre>

    For example:

    <pre>export UAA_DEFAULT_SECRET=MySecret123
export UAA_DEFAULT_USER_PW=MySecurePassword123
export UAA_DEFAULT_USER_EMAIL=dbialek@hortonworks.com
export PUBLIC_IP=172.26.231.100</pre>

    You will need to provide the email and password when logging in to the Cloudbreak web UI and when using the Cloudbreak CLI. The secret will be used by Cloudbreak for authentication.
    
    You should set the CLOUDBREAK_SMTP_SENDER_USERNAME variable to the username you use to authenticate to your SMTP server. You should set the CLOUDBREAK_SMTP_SENDER_PASSWORD variable to the password you use to authenticate to your SMTP server.

4. Generate configurations by executing:

    <pre>rm *.yml
cbd generate</pre>   

    The cbd start command includes the cbd generate command which applies the following steps:

    * Creates the `docker-compose.yml` file, which describes the configuration of all the Docker containers required for the Cloudbreak deployment.  
    * Creates the `uaa.yml` file, which holds the configuration of the identity server used to authenticate users with Cloudbreak.   

5. Start the Cloudbreak application by using the following commands:

    <pre>cbd pull-parallel
cbd start</pre>

    This will start the Docker containers and initialize the application. The first time you start the Cloudbreak app, the process will take longer than usual due to the download of all the necessary docker images.
    
    > If you encounter errors during `cbd start`, refer to [Toubleshooting](https://hortonworks.github.io/cloudbreak-documentation/latest/).  

5. Next, check Cloudbreak application logs:

    <pre>cbd logs cloudbreak</pre>

    You should see a message like this in the log: `Started CloudbreakApplication in 36.823 seconds.` Cloudbreak normally takes less than a minute to start.  


### Access Cloudbreak web UI

Log in to the Cloudbreak UI using the following steps.

**Steps** 

1. You can log into the Cloudbreak application at `https://IP_Address`. For example `https://34.212.141.253`. You may use `cbd start` to obtain the login information. Alternatively, you can obtain the VM's IP address from your cloud provider console. 


2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.
        
    | Browser | 	Steps |
|---|---|
| Firefox |Click **Advanced** > Click **Add Exception...** > Click **Confirm Security Exception** |
| Safari | Click **Continue** |
| Chrome | Click **Advanced** > Click **Proceed...** |

3. The login page is displayed:

    <img src="../images/cb_cb-ui.png" width="650" title="Cloudbreak web UI">  
    
    
4. Log in to the Cloudbreak web UI using the credentials that you configured in your `Profile` file:

    * The username is the `UAA_DEFAULT_USER_EMAIL`     
    * The password is the `UAA_DEFAULT_USER_PW` 

5. Upon a successful login, you are redirected to the dashboard:

    <img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI">  
    

### Configure external Cloudbreak database

By default, Cloudbreak uses an embedded PostgreSQL database to persist data related to Cloudbreak configuration, setup, and so on. For a production Cloudbreak deployment, you must [configure an external database](https://hortonworks.github.io/cloudbreak-documentation/latest/).


