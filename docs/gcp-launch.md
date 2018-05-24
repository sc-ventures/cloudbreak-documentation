## Launching Cloudbreak on GCP

Before launching Cloudbreak on Google Cloud, review and meet the prerequisites. Next, import Cloudbreak image, launch a VM, SSH to the VM, and start Cloudbreak. Once Cloudbreak is running, log in to the Cloudbreak UI and create a Cloudbreak credential. 

### Meet the prerequisites

Before launching Cloudbreak on GCP, you must meet the following prerequisites.

#### GCP account 

In order to launch Cloudbreak on GCP, you must log in to your GCP account. If you don't have an account, you can create one at [https://console.cloud.google.com](https://console.cloud.google.com).

Once you log in to your GCP account, you must either create a project or use an existing project. 


#### Service account

In order to launch clusters on GCP via Cloudbreak, you must have a Service Account that Cloudbreak can use to create resources. In addition, you must also have a P12 key associated with the account. The service account must have the following roles are enabled:

* Compute Engine > Compute Image User   
* Compute Engine > Compute Instance Admin (v1)  
* Compute Engine > Compute Network Admin  
* Compute Engine > Compute Security Admin  
* Storage > Storage Admin 
    
A user with an "Owner" role can assign roles to new and existing service accounts from **IAM & admin** > **Service accounts**, as presented in the following screenshots: 

<a href="../images/cb_gcp-iam.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-iam.png" width="650" title="GCP Console"></a> 

<a href="../images/cb_gcp-iam2.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-iam2.png" width="650" title="GCP Console"></a> 

For more information on creating a Service Account and generating a P12 key, refer to [GCP documentation](https://cloud.google.com/storage/docs/authentication#service_accounts). 

**Related links**  
[Service account credentials](https://cloud.google.com/storage/docs/authentication#service_accounts) (External)  

#### SSH key pair 

[Generate a new SSH key pair](faq.md#generate-ssh-key-pair) or use an existing SSH key pair. You will be required to provide it when launching the VM. 

#### Region and zone 

Decide in which region and zone you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones).  

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it. 

**Related links**  
[Regions and zones](https://cloud.google.com/compute/docs/regions-zones/) (External)  


#### VPC network 

When launching Cloudbreak, you will be required to select an existing network in which Cloudbreak can be placed. The following ports must be open on the security group: 22 (for access via SSH), 80 (for access via HTTP), and 443 (for access via HTTPS). You may use the *default* network as long as the aforementioned ports are open. 

You can manage networks under **Networking** > **VPC Networks**. To edit ports, click on the network name and then click on **Add firewall rules**. 


### Launch Cloudbreak from template     

TBD 


### Access Cloudbreak web UI

Log in to the Cloudbreak UI using the following steps.

**Steps**

1. You can log into the Cloudbreak application at `https://IP_Address`. For example `https://34.212.141.253`. You can obtain the VM's IP address from **Compute Engine** > **VM Instances**, the **External IP** column.

{!docs/common/launch-access-ui.md!} 
    
3. The login page is displayed:

    <a href="../images/cb_cb-ui.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui.png" width="650" title="Cloudbreak web UI"></a>  
    
4. Log in to the Cloudbreak web UI using the credential that you configured in your `Profile` file when [launching Cloudbreak deployer from an image](#launch-cloudbreak-deployer-from-an-image):

    * The username is the `UAA_DEFAULT_USER_EMAIL`     
    * The password is the `UAA_DEFAULT_USER_PW` 

5. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a>  

### Create Cloudbreak credential

Cloudbreak works by connecting your GCP account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a Cloudbreak credential.

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Google Cloud Platform":

    <a href="../images/cb_cb-gcp-cred.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-gcp-cred.png" width="650" title="Cloudbreak web UI"></a>  

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
