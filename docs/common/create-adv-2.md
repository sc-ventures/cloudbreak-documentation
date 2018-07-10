#### Choose Image Catalog

By default, **Choose Image Catalog** is set to the default image catalog that is provided with Cloudbreak. If you would like to use a different image catalog, you must first create and register it. For complete instructions, refer to [Custom Images](images.md).

**Related Links**     
[Custom Images](images.md)  


#### Prewarmed and Base Images 

Cloudbreak supports the following types of images for launching clusters:

| Image Type | Description | Default Images Provided | Support for Custom Images |
|---|---|---|---|
| **Base Images** | Base images include default configuration and default tooling. These images include the operating system but do not include Ambari or HDP software. | Yes | Yes | 
| **Prewarmed Images** | By default, Cloudbreak launches clusters from prewarmed images. Prewarmed images include the operating system as well as Ambari and HDP. The HDP and Ambari version used by prewarmed images cannot be customized. | Yes | No |

By default, Cloudbreak uses the included default **prewarmed images**, which include the operating system, as well as
Ambari and HDP packages installed. You can optionally select the **base image** option if you would like to:

* Use an Ambari and HDP versions different than what the prewarmed image includes and/or  
* Choose a previously created custom base image

**Choose Image**  

If under [Choose Image Catalog](#choose-image-catalog), you selected a custom image catalog, under **Choose Image** you can select an image from that catalog. For complete instructions, refer to [Custom Images](images.md). 

If you are trying to customize Ambari and HDP versions, you can ignore the **Choose Image** option; in this case default base image is used.

**Ambari Repository Specification**

If you would like to use a custom Ambari version, provide the following information: 

<div class="note">
<p class="first admonition-title"> Ambari 2.6.1</p>
<p class="last">If you would like to use Ambari <b>2.6.1</b>, use the version provided by default in the Cloudbreak web UI, or newer.</p></div>

| Parameter | Description | Example |
|---|---|---|
| Version | Enter Ambari version. | 2.6.1.3 |
| Repo Url | Provide a URL to the Ambari version repo that you would like to use. | http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.6.1.3 |
| Repo Gpg Key Url | Provide a URL to the repo GPG key. Each stable RPM package that is published by CentOS Project is signed with a GPG signature. By default, yum and the graphical update tools will verify these signatures and refuse to install any packages that are not signed, or have an incorrect signature. | http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins | 

**HDP Repository Specification**

If you would like to use a custom HDP version, provide the following information: 

| Parameter | Description | Example | 
|---|---|--|
| Stack | Stack name. | HDP |
| Version | Stack version. | 2.6 |
| OS | Operating system. | centos7 (Azure, GCP, OpenStack) or amazonlinux (AWS) |
| Repository Version | Enter repository version. | 2.6.4.0-91 |
| Version Definition File | Enter the URL of the VDF file. | http://public-repo-1.hortonworks.com/HDP/centos6/2.x/updates/2.6.4.0/HDP-2.6.4.0-91.xml |
| Enable Ambari Server to download and install GPL Licensed LZO packages? | (Optional, only available if using Ambari 2.6.1.0 or newer) Use this option to enable LZO compression in your HDP cluster. LZO is a lossless data compression library that favors speed over compression ratio. Ambari does not install nor enable LZO compression libraries by default, and must be explicitly configured to do so. For more information, refer to [Enabling LZO](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-administration/content/enabling_lzo.html).| <img src="../images/toggle.png" alt="On" /> |

> If you choose to use a base image with custom Ambari and/or HDP version, Cloudbreak validates the information entered. When Cloudbreak detects that the information entered is incorrect, it displays a warning marked with the <img src="../images/warning.png" width="25" title="Icon"> sign. You should review all the warnings before proceeding and make sure that the information that you entered is correct. If you choose to proceed in spite of the warnings, check "Ignore repository warnings".  

**Related Links**      
[Custom Images](images.md)      


#### Enable Lifetime Management 

Check this option if you would like your cluster to be automatically terminated after a specific amount of time (defined as "Time to Live" in minutes). 

[Comment]: <> (Need to Update this based on pending changes and specify: When does Cloudbreak start counting the TTL, does the time when the cluster is stopped count towards this TTL.)


#### Tags

You can optionally add tags, which will help you find your cluster-related resources, such as VMs, in your cloud provider account. refer to [Resource Tagging](tags.md).

**Related Links**      
[Resource Tagging](tags.md) 
   
   

#### Storage

You can optionally specify the following storage options for your cluster:

| Parameter | Description |
|---|---|