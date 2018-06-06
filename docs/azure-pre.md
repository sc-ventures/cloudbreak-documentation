## Meet the prerequisites on Azure 

Before launching Cloudbreak on Azure, you must meet the following prerequisites.

### Azure account

In order to launch Cloudbreak on the Azure, log in to your existing Microsoft Azure account. If you don't have an account, you can set it up at [https://azure.microsoft.com](https://azure.microsoft.com).

### Azure region

Decide in which Azure region you would like to launch Cloudbreak. You can launch Cloudbreak and provision your clusters in all regions [supported by Microsoft Azure](https://azure.microsoft.com/en-us/regions/).

Clusters created via Cloudbreak can be in the same or different region as Cloudbreak; when you launch a cluster, you select the region in which to launch it.

**Related links**  
[Azure regions](https://azure.microsoft.com/en-us/regions/) (External)


### SSH key pair

When launching Cloudbreak, you will be required to provide your public SSH key. If needed, you can generate a new SSH key pair:

* On MacOS X and Linux using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`  
* On Windows using [PuTTygen](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows)


<div class="next">
<a href="../azure-launch/index.html">Next: Launch Cloudbreak</a>
</div>
