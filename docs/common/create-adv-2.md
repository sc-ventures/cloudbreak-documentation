#### Choose Image Catalog

By default, **Choose Image Catalog** is set to the default image catalog that is provided with Cloudbreak. If you would like to use a different image catalog, you must first create and register it. For complete instructions, refer to [Custom Images](images.md).

**Related Links**     
[Custom Images](images.md)  

#### Prewarmed Images 

By default, Cloudbreak uses prewarmed images, which include the operating system, as well as Ambari [version] and HDP [version]. If you would like to use different Ambari and HDP versions, use the base image instead of a prewarmed image. Refer to [Base Images](#base-images). 


#### Base Images  

Select the **Base Image** option if you would like to:

* Use custom Ambari and HDP versions and/or  
* Choose a previously created custom base image   

<div class="note">
    <p class="first admonition-title">Supported Ambari and HDP Versions</p>
    <p class="last">
Cloudbreak supports the following HDP and Ambari versions:<ul><li>HDP <b>2.5.x</b> and HDP <b>2.6.x</b></li><li>Ambari <b>2.5.x</b>.</li></ul>Ambari 2.6.x is not supported.  
</p>
</div>

[Comment]: <> (Removed: If you would like to use Ambari <b>2.6.x</b>, use the version provided by default in the Cloudbreak web UI, or newer. Other versions are not supported.)

**Choose Image**  

If under [Choose Image Catalog](#choose-image-catalog), you selected a custom image catalog, under **Choose Image** you can select an image from that catalog. For complete instructions, refer to [Custom Images](images.md). 

If you are trying to customize Ambari and HDP versions, you can ignore the **Choose Image** option; in this case default base image is used.

**Ambari Repository Specification**

If you would like to use a custom Ambari version, provide the following information: 

| Parameter | Description | Example |
|---|---|---|
| Version | Ambari version. | 2.5.1.0 |
| Repo Url | URL to the Ambari version repo. | http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.5.1.0 |
| Repo Gpg Key Url | URL to the repo GPG key. Each stable RPM package that is published by CentOS Project is signed with a GPG signature. By default, yum and the graphical update tools will verify these signatures and refuse to install any packages that are not signed, or have an incorrect signature. | http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins | 

**HDP Repository Specification**

If you would like to use a custom HDP version, provide the following information: 

| Parameter | Description | Example | 
|---|---|--|
| Stack | Stack name. | HDP |
| Version | Stack version. | 2.6 |
| OS | Operating system. | centos7 |
| Stack Repo Id | Identifier for the repo linked in "Base Url". | HDP-2.6 |
| Base Url | URL to the repo storing the desired stack version. | http://public-repo-1.hortonworks.com/HDP/centos7/2.x/updates/2.6.1.0 |
| Utils Repo Id | Identifier for the repo linked in "Utils Base Url". | HDP-UTILS-1.1.0.21|
| Utils Base Url | URL to the repo storing utilities for the desired stack version. | http://public-repo-1.hortonworks.com/HDP-UTILS-1.1.0.21/repos/centos7 |  

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