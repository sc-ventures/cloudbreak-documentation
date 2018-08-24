
## Quickstart on GCP     

This quickstart documentation will help you get started with Cloudbreak. 

### Prerequisites

Prior to launching Cloudbreak, you must meet these prerequisites.

#### Cloud SDK 

In order to use the Cloud Deployment Manager, you must install the Google Cloud SDK on your machine. The SDK contains the gcloud CLI tool, which is used to deploy Cloudbreak. 

For instructions, refer to [Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) in the Google Cloud documentation. Make sure to perform all of the steps and validate that the `gcloud` command works on your computer. Only after validating, proceed to the next step.  

**Related links**  
[Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) (External)  
  

#### GCP APIs 
    
You must enable the **Compute Engine API** and the **Cloud Runtime Configuration API** services.

**Steps** 

1. In GCP web console, from the services menu, select **APIs & Services**: 

    <a href="../images/cb_gcp-api-01.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-api-01.png" width="650" title="Cloudbreak web UI"></a>

2. Click on **Enable APIs and services**:

    <a href="../images/cb_gcp-api-02.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-api-02.png" width="650" title="Cloudbreak web UI"></a>

3. On this page:
    * In the filter, type "Compute Engine API".  
    * Click on the corresponding tile to navigate to the API details   

    <a href="../images/cb_gcp-api-03.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-api-03.png" width="650" title="Cloudbreak web UI"></a>  
    
3. Click on the **Enable** button. Once the API has been enabled you should see:

    <a href="../images/cb_gcp-api-04.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-api-04.png" width="650" title="Cloudbreak web UI"></a>

4. Perform the same for the "Cloud Runtime Configuration API". 

    
#### Service account        
    
Create a service account that has the following roles:

Computer Engine roles: 

* Compute Image User     
* Compute Instance Admin (v1)
* Compute Network Admin  
* Compute Security Admin  

Storage roles:

* Storage > Storage Admin 

Other roles: 

* Cloud RuntimeConfig Admin (You can find it under "Other")

If you already have a service account and a JSON key but you need to update the permissions for the account, you can do it from **IAM & admin** > **IAM**. If you need to create a service account, follow these steps. 

**Steps**

1. To create a service account In GCP web console, from the services menu, select **IAM & admin** > **Service account**:

    <a href="../images/cb_gcp-service-01.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-service-01.png" width="650" title="Cloudbreak web UI"></a>  

2. Click on **Create service account**:

    <a href="../images/cb_gcp-service-02.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-service-02.png" width="650" title="Cloudbreak web UI"></a>  

4. Provide the following:

    * Enter the **Service account name**.   
    > This will determine your service account email. Make a note of this service account email. You will need to provide it when creating a Cloudbreak credential.       
    * Under **Role**, select the roles described above.  
    * Under **Key type**, select **JSON**.    

    <a href="../images/cb_gcp-service-03.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-service-03.png" width="500" title="Cloudbreak web UI"></a>  

5. Click **Create**.   

6. The JSON key will be downloaded on your machine. You will need it later to create a Cloudbreak credential.  
     
    
### Launch from the quickstart template 

Launch Cloudbreak from an Cloud Deployment Manager template by using the following steps. This is the quickstart deployment option. 

**Steps**       
    
1. Log in to your GitHub account.  

1. Run the following command to download the following Hortonworks repo onto your computer and check out the release branch:

    <pre>git clone https://github.com/hortonworks/cbd-quickstart
cd cbd-quickstart
git checkout 2.7.1</pre> 

[Comment]: <> (Should we tell people to fork this or to download the repo?)

1. On your computer, browse to the `cbd-quickstart/gcp`.         

1. Open the vm_template_config.yaml file in a text editor. 

1. Edit the file by updating the property values: 

    > Do not edit any other parameters in the vm_template_config.yaml file. 

    | Parameter | Description | Default |
|---|---|---|
| region | The GCP region in which you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones). | us-central-1 |
| zone | The GCP region's zone in which you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones).| us-central1-a |
| instance_type | Select the VM instance type. For available instance types, reefer to [Machine Types](https://cloud.google.com/compute/docs/machine-types) in GCP documentation. |  n1-standard-4 |
| ssh_pub_key | Paste your SSH public key. | ... |
| os_user | Enter the name of the user that you would like to use to SSH to the VM. |  cloudbreak |
| user_email | Enter the email address that you would like to use to log in to Cloudbreak. | admin@cloudbreak.com |
| user_password | Enter the password that you would like to use to log in to Cloudbreak. | cloudbreak |
| service_account_email | The email for the service account created in prerequisites. | ... |

[Comment]: <> (VPC not available?)

1. Save the changes on your local machine. 

1. Run the following command to create a new deployment:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=/path/to/your/file/vm_template_config.yaml</pre>
    
    For example:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=/Users/youruser/Documents/cbd-quickstart/gcp/vm_template_config.yaml</pre>
    
1. Once your deployment has finished, you will see the following:

    <pre>gcloud deployment-manager deployments create cbd-deployment --config=/Users/youruser/Documents/cbd-quickstart/gcp/vm_template_config.yaml
Waiting for create operation-1527749967574-56d7b021f73f1-773609ee-060d4332...done.
Create operation operation-1527749967574-56d7b021f73f1-773609ee-060d4332 completed successfully.
NAME                            TYPE                          STATE      ERRORS  INTENT
cbd-deployment-default-route-1  compute.v1.route              COMPLETED  []
cbd-deployment-network          compute.v1.network            COMPLETED  []
cbd-deployment-startup-config   runtimeconfig.v1beta1.config  COMPLETED  []
cbd-deployment-startup-waiter   runtimeconfig.v1beta1.waiter  COMPLETED  []
cbd-deployment-subnet           compute.v1.subnetwork         COMPLETED  []
cbd-deployment-vm               compute.v1.instance           COMPLETED  []
firewall-cbd-deployment         compute.v1.firewall           COMPLETED  []
OUTPUTS       VALUE
deploymentIp  35.224.36.96</pre>

    <div class="note">
    <p class="first admonition-title">Cleaning up after a failed deployment</p>
    <p class="last">For steps on how to delete Cloudbreak after a failed deployment, refer to <a href="../cb-delete/index.html#delete-cloudbreak-on-gcp">Delete Cloudbreak on GCP</a>.</p>
</div>

1. The last output should be the the deploymentIp. Copy the IP address and paste it in the browser so that you can log in to the Cloudbreak web UI.                   

{!docs/common/launch-access-ui.md!} 
    
1. Log in to the Cloudbreak web UI using the credentials that you configured in the vm_template_config.yaml file.  

1. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a> 


**Related Links**  
[Machine types](https://cloud.google.com/compute/docs/machine-types) (External)    
[Regions and zones](https://cloud.google.com/compute/docs/regions-zones/) (External)     
   

### Create Cloudbreak credential

Cloudbreak works by connecting your GCP account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](concepts.md#cloudbreak-credential).  

<a href="https://youtu.be/uVYpgz9m4eE" target="_blank" title="Click to open"><img src="../images/cb_video-placeholder.png" width="450" title="YouTube video"></a>

**Prerequisites**

As described in the [Service account](#service-account) section in the prerequisites, in order to launch clusters on GCP via Cloudbreak, you must have a service account that Cloudbreak can use to create resources.   

{!docs/common/gcp-cred.md!}

**Related links**  
[Cloudbreak credential](concepts.md#cloudbreak-credential)  
[Service account](#service-account) 

### Create a cluster     

{!docs/common/create-quick.md!}
