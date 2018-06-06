## Meet the prerequisites on GCP 

Before launching Cloudbreak on GCP, you must meet the following prerequisites.

### GCP account 

In order to launch Cloudbreak on GCP, you must log in to your GCP account. If you don't have an account, you can create one at [https://console.cloud.google.com](https://console.cloud.google.com).

Once you log in to your GCP account, you must either create a project or use an existing project. 

### SSH key pair 

[Generate a new SSH key pair](faq.md#generate-ssh-key-pair) or use an existing SSH key pair. You will be required to provide it when launching the VM. 

### Region and zone 

Decide in which region and zone you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by GCP](https://cloud.google.com/compute/docs/regions-zones/regions-zones).  

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it. 

**Related links**  
[Regions and zones](https://cloud.google.com/compute/docs/regions-zones/) (External) 


<div class="next">
<a href="../gcp-launch/index.html">Next: Launch Cloudbreak</a>
</div>
