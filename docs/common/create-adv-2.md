#### Choose OS Type

By default, the **Choose OS Type** is set to the default image that is provided with Cloudbreak. If you would like to use a different image, you must register it in a custom image catalog, as described in the [Custom Images](images.md) documentation.

**Related Links**   
[Custom Images](images.md)


#### Base Images  

The **Base Image** option allows you to customize Ambari and/or HDP version used. 

*Ambari Parameters*

| Parameter | Description | Default Value |
|---|---|---|
| Version | Ambari version. | 2.5.1.0 |
| Repo Url | URL to the Ambari version repo. | http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.5.1.0 |
| Repo Gpg Key Url | URL to the repo GPG key. Each stable RPM package that is published by CentOS Project is signed with a GPG signature. By default, yum and the graphical update tools will verify these signatures and refuse to install any packages that are not signed, or have an incorrect signature. | http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins | 

*HDP Parameters*

| Parameter | Description | Default Value | 
|---|---|--|
| Stack | Stack name. | HDP |
| Version | Stack version. | 2.6 |
| OS | Operating system. | centos7 |
| Stack Repo Id | Identifier for the repo linked in "Base Url". | HDP-2.6 |
| Base Url | URL to the repo storing the desired stack version. | http://public-repo-1.hortonworks.com/HDP/centos7/2.x/updates/2.6.1.0 |
| Utils Repo Id | Identifier for the repo linked in "Utils Base Url". | HDP-UTILS-1.1.0.21|
| Utils Base Url | URL to the repo storing utilities for the desired stack version. | http://public-repo-1.hortonworks.com/HDP-UTILS-1.1.0.21/repos/centos7 |


#### Enable Lifetime Management 

Check this option if you would like your cluster to be automatically terminated after a specific amount of time (defined as "Time to Live" in minutes) has passed. 

#### Tags

You can optionally add tags, which will help you find your cluster-related resources, such as VMs, in your cloud provider account. refer to [Resource Tagging](tags.md).

**Related Links**    
[Resource Tagging](tags.md)  


#### Storage

You can optionally specify the following storage options for your cluster:

| Parameter | Description |
|---|---|