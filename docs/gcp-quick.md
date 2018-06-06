
## (Quickstart) Launch Cloudbreak from a template    

Follow these steps to launch Cloudbreak on Google Cloud by using the Cloud Deployer Manager. 

[Comment]: <> (https://cloud.google.com/deployment-manager/) 

### Prerequisites

1. In order to use the Cloud Deployment Manager, you must install the Google Cloud SDK on your machine. The SDK contains the gcloud CLI tool, which is used to deploy Cloudbreak. 

    For instructions, refer to [Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) in the Google Cloud documentation. Make sure to perform all of the steps and validate that the `gcloud` command works on your computer. Only after validating, proceed to the next step.  
    
2. Enable the **Compute Engine API** and the **Cloud Runtime Configuration API** services. 

    To enable these, from services menu, select **APIs & Services**. Next, click on **ENABLE APIS AND SERVICES**, type each service name in the filter, and enable it.    
    
3. Create a service account that has read and write permissions to:
    * Compute Image    
    * Compute Instance  
    * Compute Network  
    * Compute Security  
    * Cloud RuntimeConfig. 

    The service account email needs to specified in the config.yaml (or in the gcloud command) as a property.     
    
### Launch from a template        
    
1. Log in to GitHub (https://github.com/).  

1. Download the "cbd-quickstart" Hortonworks repo located at `https://github.com/hortonworks/cbd-quickstart/tree/rc-2.7` onto your computer. 

[Comment]: <> (Should we tell people to fork this or to download the repo?)

[Comment]: <> (Will we be creating an RC branch for each release and pointing everyone to that branch?)

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
    <p class="last">For steps on how to delete Cloudbreak after a failed deployment, refer to <a href="../cb-delete/index.html#delete-cloudbreak-on-gcp">Delete Cloudbreak on GCP</a></p>
</div>

1. The last output should be the the deploymentIp. Copy the IP address and paste it in the browser so that you can log in to the Cloudbreak web UI.                   

{!docs/common/launch-access-ui.md!} 
    
1. Log in to the Cloudbreak web UI using the credentials that you configured in the vm_template_config.yaml file.  

1. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a> 


**Related Links**  
[Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) (External)  
[Regions and Zones](https://cloud.google.com/compute/docs/regions-zones/regions-zones) (External)    
[Machine Types](https://cloud.google.com/compute/docs/machine-types) (External)    


