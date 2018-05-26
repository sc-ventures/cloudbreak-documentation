## Launching Cloudbreak on GCP

Before launching Cloudbreak on Google Cloud:

* Review and meet the [prerequisites](#prerequisites).  
* If you haven't already, review the [deployment options](deployment-options.md).  

The steps below describe how to launch Cloudbreak by using one of the two available deployment options (quickstart and production), access Cloudbreak web UI, and create a Cloudbreak credential.

### (Quickstart) Launch Cloudbreak from a template     

Follow these steps to launch Cloudbreak on Google Cloud by using the Cloud Deployer Manager. 

[Comment]: <> (https://cloud.google.com/deployment-manager/) 

> This is the GCP quickstart option. If you would like to install Cloudbreak on your own VM, refer to [Launch Cloudbreak on your own VM](vm-launch.md). 

**Steps**

1. In order to use the Cloud Deployment Manager, you must install the Google cloud SDK on your machine. The SDK contains the gcloud CLI tool, which is used to deploy Cloudbreak. 

    For instructions, refer to [Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) in Google Cloud documentation.  

[Comment]: <> (https://cloud.google.com/sdk/gcloud/)

1. Open a text editor and paste the following into the file:  

    <pre>region: us-central1
zone: us-central1-a
instance_type: n1-standard-4
ssh_pub_key: ....
os_user: cloudbreak
user_email: admin@example.com
user_password: cloudbreak</pre>

2. Edit the file by updating the parameters: 

    | Parameter | Description | Default |
|---|---|---|
| region | The GCP region in which you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones). | us-central-1 |
| zone | The GCP region's zone in which you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones).| us-central1-a |
| instance_type | Select the VM instance type. For available instance types, reefer to [Machine Types](https://cloud.google.com/compute/docs/machine-types) in GCP docuemntation. |  n1-standard-4 |
| ssh_pub_key | Paste your SSH public key. | ... |
| os_user | Enter the name of the user that you would like to use to SSH to the VM. |  cloudbreak |
| user_email | Enter the email address that you would like to use to log in to Cloudbreak. | admin@cloudbreak.com |
| user_password | Enter the password that you would like to use to log in to Cloudbreak. | cloudbreak |

[Comment]: <> (VPC not available?)

1. Save the file on your local machine in the yaml format. 

1. Run the following command to create a new deployment:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=/path/to/your/file.yaml</pre>
    
    For example:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=vm_template_config.yaml</pre>
    
   

To delete the previously created deployment via gcloud command line interface, use:
    
<pre>gcloud deployment-manager deployments delete cbd-deployment -q</pre>


**Related Links**  
[Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) (External)  
[Regions and Zones](https://cloud.google.com/compute/docs/regions-zones/regions-zones) (External)    
[Machine Types](https://cloud.google.com/compute/docs/machine-types) (External)    


### (Production) Launch Cloudbreak on your own VM

Refer to [Launch Cloudbreak on your own VM](vm-launch.md). 


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
