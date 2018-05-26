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

1. In order to use the Cloud Deployment Manager, you must install the Google Cloud SDK on your machine. The SDK contains the gcloud CLI tool, which is used to deploy Cloudbreak. 

    For instructions, refer to [Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) in the Google Cloud documentation. Make sure to perform all of the steps and validate that the `gcloud` command works on your computer. Only after validating, proceed to the next step.     
    
1. Log in to GitHub (https://github.com/).  

1. Download the "cbd-quickstart" Hortonworks repo located at `https://github.com/hortonworks/cbd-quickstart` onto your computer. 

[Comment]: <> (Should we tell people to fork this or to download it?)

1. On your computer, browse to the `cbd-quickstart/gcp`.         

[Comment]: <> (https://cloud.google.com/sdk/gcloud/)

1. Open the vm_template_config.yaml file in a text editor. 

1. Edit the file by updating the parameter values: 

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

1. Save the changes on your local machine. 

1. Run the following command to create a new deployment:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=/path/to/your/file/vm_template_config.yaml</pre>
    
    For example:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=/Users/youruser/Documents/cbd-quickstart/gcp/vm_template_config.yaml</pre>
    
1. Once your deployment has finished, you will see the following:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=/Users/youruser/Documents/cbd-quickstart/gcp/vm_template_config.yaml
The fingerprint of the deployment is V30aruFnbg8Bu6YYNzOyff==
done.                                                                          
Create operation operation-1527295679118-56d113c6bbe99-a353e786-4e851f18 completed successfully.
NAME                            TYPE                   STATE      ERRORS  INTENT
cbd-deployment-default-route-1  compute.v1.route       COMPLETED  []
cbd-deployment-network          compute.v1.network     COMPLETED  []
cbd-deployment-subnet           compute.v1.subnetwork  COMPLETED  []
firewall-cbd-deployment         compute.v1.firewall    COMPLETED  []
vm-cbd-deployment               compute.v1.instance    COMPLETED  []</pre>
       
1. In the browser, navigate to [https://console.cloud.google.com/](https://console.cloud.google.com/) and log in to your Google Cloud account. 

1. From the navigation menu, select **Deployment Manager > Deployments**:

    <a href="../images/cb_gcp-01.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-01.png" width="650" title="GCP"></a>   
    
1. On the **Deployments** page, you should see your Cloudbreak deployment: 

    <a href="../images/cb_gcp-02.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-02.png" width="650" title="GCP"></a>   
    
1. Click on the deployment to view details. You should see the following resources:

    <a href="../images/cb_gcp-03.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-03.png" width="650" title="GCP"></a>
             
1. Click on the **vm-cbd-deployment** to see deployment details. Next, in the top right corner, click on **Manage Resource** to see VM instance details: 

    <a href="../images/cb_gcp-04.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-04.png" width="650" title="GCP"></a>
    
1. On the **VM instance details** page, scroll down to find the external IP address of the VM:

    <a href="../images/cb_gcp-05.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-05.png" width="650" title="GCP"></a>
    
1. Copy the external IP address and paste it in the browser.                   

{!docs/common/launch-access-ui.md!} 
    
1. The login page is displayed:

    <a href="../images/cb_cb-ui.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui.png" width="650" title="Cloudbreak web UI"></a>  
    
1. Log in to the Cloudbreak web UI using the credentials that you configured earlier.  

5. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a> 


**Related Links**  
[Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) (External)  
[Regions and Zones](https://cloud.google.com/compute/docs/regions-zones/regions-zones) (External)    
[Machine Types](https://cloud.google.com/compute/docs/machine-types) (External)    


### (Production) Launch Cloudbreak on your own VM

Refer to [Launch Cloudbreak on your own VM](vm-launch.md). 
 

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
