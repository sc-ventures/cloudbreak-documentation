
## Quickstart on GCP     

This quickstart documentation will help you get started with Cloudbreak. 

### Prerequisites

Prior to launching Cloudbreak, you must meet these prerequisites.

#### Cloud SDK 

In order to use the Cloud Deployment Manager, you must install the Google Cloud SDK on your machine. The SDK contains the gcloud CLI tool, which is used to deploy Cloudbreak. 

For instructions, refer to [Installing Google Cloud SDK](https://cloud.google.com/sdk/downloads) in the Google Cloud documentation. Make sure to perform all of the steps and validate that the `gcloud` command works on your computer. Only after validating, proceed to the next step.  
  

#### GCP APIs 
    
You must enable the **Compute Engine API** and the **Cloud Runtime Configuration API** services.

**Steps** 

1. In GCP web console, from the services menu, select **APIs & Services**: 

    <img src="../images/cb_gcp-api-01.png" width="650" title="Cloudbreak web UI">

2. Click on **Enable APIs and services**:

    <img src="../images/cb_gcp-api-02.png" width="650" title="Cloudbreak web UI">

3. On this page:
    * In the filter, type "Compute Engine API".  
    * Click on the corresponding tile to navigate to the API details   

    <img src="../images/cb_gcp-api-03.png" width="650" title="Cloudbreak web UI">  
    
3. Click on the **Enable** button. Once the API has been enabled you should see:

    <img src="../images/cb_gcp-api-04.png" width="650" title="Cloudbreak web UI">

4. Perform the same for the "Cloud Runtime Configuration API". 

    
#### Service account        
    
Create a service account that has the following roles:

Computer Engine roles: 

* Compute Image User     
* Compute Instance Admin 
* Compute Network Admin  
* Compute Security Admin  

Storage roles:

* Storage > Storage Admin 

Other roles: 

* Cloud RuntimeConfig Admin (You can find it under "Other")

If you already have a service account and a JSON key but you need to update the permissions for the account, you can do it from **IAM & admin** > **IAM**. If you need to create a service account, follow these steps. 

**Steps**

1. To create a service account In GCP web console, from the services menu, select **IAM & admin** > **Service account**:

    <img src="../images/cb_gcp-service-01.png" width="650" title="Cloudbreak web UI">  

2. Click on **Create service account**:

    <img src="../images/cb_gcp-service-02.png" width="650" title="Cloudbreak web UI">  

4. Provide the following:

    * Enter the **Service account name**.   
    > This will determine your service account email. Make a note of this service account email. You will need to provide it when creating a Cloudbreak credential.       
    * Under **Role**, select the roles described above.  
    * Under **Key type**, select **JSON**.    

    <img src="../images/cb_gcp-service-03.png" width="500" title="Cloudbreak web UI">  

5. Click **Create**.   

6. The JSON key will be downloaded on your machine. You will need it later to create a Cloudbreak credential.  
     
    
### Launch from the quickstart template 

Launch Cloudbreak from an Cloud Deployment Manager template by using the following steps. This is the quickstart deployment option. 

**Steps**       
    
1. Log in to your GitHub account.  

1. Run the following command to download the following Hortonworks repo onto your computer and check out the release branch:

    <pre>git clone https://github.com/hortonworks/cbd-quickstart
cd cbd-quickstart
git checkout 2.7.0</pre> 

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

1. The last output should be the the deploymentIp. Copy the IP address and paste it in the browser so that you can log in to the Cloudbreak web UI.                   


2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.
        
    | Browser | 	Steps |
|---|---|
| Firefox |Click **Advanced** > Click **Add Exception...** > Click **Confirm Security Exception** |
| Safari | Click **Continue** |
| Chrome | Click **Advanced** > Click **Proceed...** |

3. The login page is displayed:

    <img src="../images/cb_cb-ui.png" width="650" title="Cloudbreak web UI">
     
    
1. Log in to the Cloudbreak web UI using the credentials that you configured in the vm_template_config.yaml file.  

1. Upon a successful login, you are redirected to the dashboard:

    <img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI">    
   

### Create Cloudbreak credential

Cloudbreak works by connecting your GCP account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](https://hortonworks.github.io/cloudbreak-documentation/latest/).  

**Prerequisites**

As described in the [Service account](https://hortonworks.github.io/cloudbreak-documentation/latest/) section in the prerequisites, in order to launch clusters on GCP via Cloudbreak, you must have a service account that Cloudbreak can use to create resources.   


**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane. 

2. Click **Create Credential**. 

3. Under **Cloud provider**, select "Google Cloud Platform":

    <img src="../images/cb_cb-gcp-cred.png" width="650" title="Cloudbreak web UI">  

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Key type | Select JSON or P12. Since activating service accounts with P12 private keys has been deprecated in the Cloud SDK, we recommend using JSON. |
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| Project Id | (Only required for P12 key type) Enter the project ID. You can obtain it from your GCP account by clicking on the name of your project at the top of the page and copying the **ID**. |
| Service Account Email Address | (Only required for P12 key type) "Service account ID" value for your service account created in prerequisites. You can find it on GCP at **IAM & Admin** > **Service accounts**. |
| Service Account Private Key | Upload the key that you created in the prerequisites when creating a service account. |

4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to create clusters. 



### Create a cluster     


Use these steps to create a cluster. This section only covers minimal steps required for creating a cluster based on the basic settings (2-node cluster with default hardware and storage options).  

**Steps**

1. Click the **Create Cluster** button and the *Create Cluster* wizard is displayed.  
    By default, **Basic** view is displayed.

    <img src="../images/cb_quick-create01.png" width="650" title="Cloudbreak web UI">  

3. On the **General Configuration** page:

    * **Cluster Name**: Enter a name for your cluster. The name must be between 5 and 40 characters, must start with a letter, and must only include lowercase letters, numbers, and hyphens.
    * **Region**: Select the region in which you would like to launch your cluster. 
    * **Cluster Type**: Choose one of default cluster configurations.  

4. Click **Next** three times to navigate to the **Security Page**. You do not need to enter anything on the **Hardware and Storage** and **Network** because by default Cloudbreak suggests the instance types, storage, and network to use (a new network and subnet is created by default).   

5. One the **Security** page, provide the following:

    * **Cluster User**: This will be the user that you should use to log in to Ambari and other cluster UIs. By default, this is `admin`.   
    * **Cluster Password**: Password for the cluster user.  
    * **SSH public key**: Select the existing public SSH key or paste your key. The key will be placed on the cluster VMs so that you can use the matching private key to access the VMs via SSH. 

    <img src="../images/cb_quick-create02.png" width="650" title="Cloudbreak web UI">  

6. Click on **Create Cluster** to create a cluster.

7. You will be redirected to the Cloudbreak dashboard, and a new tile representing your cluster will appear at the top of the page.

 
### Next steps

To learn how to access your cluster, refer to [Accessing a cluster](https://hortonworks.github.io/cloudbreak-documentation/latest/).

To learn how to use Cloudbreak to manage your cluster, refer to  [Managing and monitoring clusters](https://hortonworks.github.io/cloudbreak-documentation/latest/).


### Clean up after a failed deployment


#### Deleting clusters  

The proper way to delete clusters is to use the the **Terminate** option available in the Cloudbreak UI. If the terminate process fails, try the **Terminate** > **Force terminate** option.

If the force termination does not delete all cluster resources, delete the resources manually:

* To find the VMs, click on the links available in the cluster details. 
* To find the network and subnet, see the **Cluster Information** in the cluster details. 

Upon cluster termination, Cloudbreak only terminates the resources that it created. It does not terminate any resources (such as networks, subnets, roles, and so on) which existed prior to cluster creation. 


#### Delete Cloudbreak on GCP 

There are two ways to delete a previously created Cloudbreak deployment from your Google Cloud account. 

(Option 1) You can delete the deployment from the Google Cloud console in your browser, from the **Deployment Manager > Deployments**:

<img src="../images/cb_gcp-delete.png" width="650" title="GCP">

(Option 2) You can delete the deployment by using the following gcloud CLI command:

<pre>gcloud deployment-manager deployments delete deployment-name -q</pre>

For example: 
    
<pre>gcloud deployment-manager deployments delete cbd-deployment -q</pre> 


