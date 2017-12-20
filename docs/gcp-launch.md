## Launching Cloudbreak on GCP

Before launching Cloudbreak on Google Cloud, review and meet the prerequisites. Next, import Cloudbreak image, launch a VM, SSH to the VM, and start Cloudbreak. Once Cloudbreak is running, log in to the Cloudbreak UI and create a Cloudbreak credential. 

### Meet the Prerequisites

Before launching Cloudbreak on GCP, you must meet the following prerequisites.

#### GCP Account 

In order to launch Cloudbreak on GCP, you must log in to your GCP account. If you don't have an account, you can create one at [https://console.cloud.google.com](https://console.cloud.google.com).

Once you log in to your GCP account, you must either create a project or use an existing project. 


#### Service Account

In order to launch clusters on GCP via Cloudbreak, you must have a Service Account that Cloudbreak can use to create resources. In addition, you must also have a P12 key associated with the account. If you need to create these, refer to [GCP documentation](https://cloud.google.com/storage/docs/authentication#service_accounts) on how to create a service account and generate a P12 key. 

Once you have the service account that you want to use for Cloudbreak, make sure that your service account fulfills one of the following APIs are enabled for your service account:

* Compute Image User   
* Compute Instance Admin (v1)  
* Compute Network Admin  
* Compute Security Admin  
    
A user with an "Owner" role can assign roles or access rules to service accounts from **IAM & Admin** > **IAM**. For example:

<a href="../images/gcp-iam.png" target="_blank" title="click to enlarge"><img src="../images/gcp-iam.png" width="650" title="GCP Console"></a> 

**Related Links**  
[Service Account Credentials](https://cloud.google.com/storage/docs/authentication#service_accounts) (External)  

#### SSH Key Pair 

[Generate a new SSH key pair](faq.md#generate-ssh-key-pair) or use an existing SSH key pair. You will be required to provide it when launching the VM.  


#### VPC Network 

When launching Cloudbreak, you will be required to select an existing network in which Cloudbreak can be placed. The following ports must be open on the security group: 22 (for access via SSH), 80 (for access via HTTP), and 443 (for access via HTTPS). You may use the *default* network as long as the aforementioned ports are open. 

You can manage networks under **Networking** > **VPC Networks**. To edit ports, click on the network name and then click on **Add firewall rules**.

#### Region and Zone 

Decide in which region and zone you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones).  

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it. 

**Related Links**  
[Regions and Zones](https://cloud.google.com/compute/docs/regions-zones/) (External)  


### Launch the VM  

**Steps**

1. Log in to Google Cloud Platform.

1. Open the **Google Cloud Shell** by clicking on the  <img src="../images/gcp-icon.png" width="25" title="Icon"> icon in the top-right corner:

    <a href="../images/gcp-01.png" target="_blank" title="click to enlarge"><img src="../images/gcp-01.png" width="650" title="GCP Console"></a> 

1. Import the Cloudbreak deployer image by executing the following command: 

    <pre>gcloud compute images create cloudbreak-deployer-220-2017-12-19 --source-uri gs://sequenceiqimage/cloudbreak-deployer-220-2017-12-19.tar.gz</pre>

[comment]: <> (TO-DO: This should be generated automatically.) 
    
1. In the GCP UI, from the **Products and services** menu, select **Compute Engine** > **Images**.

1. In the search bar, type the name of the Cloudbreak deployer image that you imported earlier.

1. Select the image and then select **Create Instance**:  

    <a href="../images/gcp-02.png" target="_blank" title="click to enlarge"><img src="../images/gcp-02.png" width="650" title="GCP Console"></a>  

1. You will be redirected to **VM instances** > **Create an instance** form. Provide the following parameters for your VM:

    <a href="../images/gcp-03.png" target="_blank" title="click to enlarge"><img src="../images/gcp-03.png" width="650" title="GCP Console"></a>  

    | Parameter | Description |
|---|---|
| Name | Enter a name for the VM. |
| Zone | Select the zone in which to launch the VM. |
| Machine type | The minimum instance type suitable for Cloudbreak is **n1-standard-2**. The minimum requirements are 4GB RAM, 10GB disk, 2 cores. |
| Boot disk | Verify that the Cloudbreak deployer disk which you imported earlier is pre-selected. |

1. Click on **Management, disks, networking, SSH keys** to view the options.

1. Under **Networking** > **Network interfaces**, select the network in which you want to launch Cloudbreak. 

1. Under **SSH Keys**, check **Block project-wise SSH keys** and paste your public SSH key.

1. Click **Create**. 


### SSH to the VM

Now that your VM is ready, access it via SSH: 
  
* Use a private key matching the public key that you added to your  project.
* The SSH user is called "cloudbreak".
* You can obtain the VM's IP address from **Compute Engine** > **VM Instances**, the **External IP** column.

On Mac OS X, you can SSH to the VM by running the following from the Terminal app: `ssh -i "your-private-key.pem" cloudbreak@instance_IP` where "your-private-key.pem" points to the location of your private key and "instance_IP" is the public IP address of the VM.

On Windows, you can use [PuTTy](http://www.putty.org/).


### Launch Cloudbreak Deployer

After accessing the VM via SSH, launch Cloudbreak deployer using the following steps.

**Steps** 

1. Navigate to the cloudbreak-deployment directory:

    <pre>cd /var/lib/cloudbreak-deployment/</pre>
    
    This directory contains configuration files and the supporting binaries for Cloudbreak deployer.
    
2.  Initialize your profile by creating a new file called `Profile` and adding the following content:

    <pre>export UAA_DEFAULT_SECRET=MY-SECRET
export UAA_DEFAULT_USER_PW=MY-PASSWORD
export UAA_DEFAULT_USER_EMAIL=MY-EMAIL</pre>  

    For example: 

    <pre>export UAA_DEFAULT_SECRET=MySecret123
export UAA_DEFAULT_USER_PW=MySecurePassword123
export UAA_DEFAULT_USER_EMAIL=dbialek@hortonworks.com</pre> 

    > You will need to provide the email and password when logging in to the Cloudbreak web UI and when using the Cloudbreak CLI. The secret will be used by Cloudbreak for authentication.  
    
3. Start the Cloudbreak application by using the following command:

    <pre>cbd start</pre>

    This will start the Docker containers and initialize the application. The first time you start the Coudbreak app, this also downloads of all the necessary docker images.
    
[Comment]: <> (Extra info which may not be needed here: The `cbd start` command includes the `cbd generate` command which applies the following steps: Creates the `docker-compose.yml` file, which describes the configuration of all the Docker containers needed for the Cloudbreak deployment. Creates the `uaa.yml` file, which holds the configuration of the identity server used to authenticate users with Cloudbreak.)

    Once the `cbd start` has finished, it returns the "Uluwatu (Cloudbreak UI) url" which you can later paste in your browser and log in to Cloudbreak web UI.

4. Check Cloudbreak deployer version and health: 
    
    <pre>cbd doctor</pre>
    
5. Next, check Cloudbreak Application logs: 

    <pre>cbd logs cloudbreak</pre>
    
    You should see a message like this in the log: `Started CloudbreakApplication in 36.823 seconds.` Cloudbreak takes less than a minute to start. If you try to access the Cloudbreak UI before Cloudbreak started, you will get a "Bad Gateway" error or "Cannot connect to Cloubdreak" error.
     

### Access Cloudbreak UI

Log in to the Cloudbreak UI using the following steps.

**Steps**

1. You can log into the Cloudbreak application at `https://IP_Address`. For example `https://34.212.141.253`. You can obtain the VM's IP address from **Compute Engine** > **VM Instances**, the **External IP** column.

2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.
    
3. The login page is displayed:

    <a href="../images/cb-ui.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui.png" width="650" title="Cloudbreak web UI"></a>  
    
4. Log in to the Cloudbreak web UI using the credential that you configured in your `Profile` file when [launching Cloudbreak deployer](#launch-cloudbreak-deployer):

    * The username is the `UAA_DEFAULT_USER_EMAIL`     
    * The password is the `UAA_DEFAULT_USER_PW` 

5. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui1.png" width="650" title="Cloudbreak web UI"></a>  

### Create Cloudbreak Credential

Cloudbreak works by connecting your GCP account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a Cloudbreak credential.

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Google Cloud Platform":

    <a href="../images/cb-gcp-cred.png" target="_blank" title="click to enlarge"><img src="../images/cb-gcp-cred.png" width="650" title="Cloudbreak web UI"></a>  

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| Project Id | Enter the project ID. You can obtain it from your GCP account by clicking on the name of your project at the top of the page and copying the **ID**. |
| Service Account Email Address | "Service account ID" value for your service account created in prerequisites. You can find it on GCP at **IAM & Admin** > **Service accounts**. |
| Service Account Private (p12) Key | Upload the P12 key that you created in the prerequisites when creating a service account. |

4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to [create clusters](gcp-create.md). 



<div class="next">
<a href="../gcp-create/index.html">Next: Create a Cluster</a>
</div>
