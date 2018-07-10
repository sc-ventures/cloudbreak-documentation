## Prerequisites on GCP 

Before launching Cloudbreak on GCP, you must meet the following prerequisites.

### GCP Account 

In order to launch Cloudbreak on GCP, you must log in to your GCP account. If you don't have an account, you can create one at [https://console.cloud.google.com](https://console.cloud.google.com).

Once you log in to your GCP account, you must either create a project or use an existing project. 

### Service Account

In order to launch clusters on GCP via Cloudbreak, you must have a service account that Cloudbreak can use to create resources. In addition, you must also have a JSON key associated with the account. 

The service account must have the following roles are enabled:

* Compute Engine > Compute Image User   
* Compute Engine > Compute Instance Admin (v1)  
* Compute Engine > Compute Network Admin  
* Compute Engine > Compute Security Admin  
* Storage > Storage Admin 
    
A user with an "Owner" role can assign roles to new and existing service accounts from **IAM & admin** > **Service accounts**, as presented in the following screenshots: 

<a href="../images/cb_gcp-iam.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-iam.png" width="650" title="GCP Console"></a> 

<a href="../images/cb_gcp-iam2.png" target="_blank" title="click to enlarge"><img src="../images/cb_gcp-iam2.png" width="650" title="GCP Console"></a> 

For more information on creating a service account and generating a JSON key, refer to [GCP documentation](https://cloud.google.com/storage/docs/authentication#service_accounts). 

**Related links**  
[Service account credentials](https://cloud.google.com/storage/docs/authentication#service_accounts) (External) 

### SSH Key Pair 

[Generate a new SSH key pair](faq.md#generate-ssh-key-pair) or use an existing SSH key pair. You will be required to provide it when launching the VM. 

{!docs/common/vm-pre.md!}

### Region and Zone 

Decide in which region and zone you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones).  

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it. 

**Related links**  
[Regions and zones](https://cloud.google.com/compute/docs/regions-zones/) (External) 


## Launching Cloudbreak on GCP

These steps describe how to launch Cloudbreak on GCP for production. 
Before launching Cloudbreak on Google Cloud, review and meet the [prerequisites](#prerequisites). Next, follow the steps below.  


{!docs/common/vm-req.md!}

{!docs/common/vm-launch.md!}
 

### Create Cloudbreak Credential

Cloudbreak works by connecting your GCP account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](concepts.md#cloudbreak-credential).  

**Prerequisites**

In order to launch clusters on GCP via Cloudbreak, you must have a service account that Cloudbreak can use to create resources. If you do not have one, refer to [Prerequisites: Service account](#service-account).  

{!docs/common/gcp-cred.md!}


<div class="next">
<a href="../gcp-create/index.html">Next: Create a Cluster</a>
</div>
