#### Base Image

The **Base Image** option allows you to customize Ambari and/or HDP version used. 

*Ambari Parameters*

| Parameter | Description | Example |
|---|---|---|
| Version | Enter Ambari version. | 2.5.1.0 |
| Repo Url | | http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.5.1.0 |
| Repo Gpg Key Url | | http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins | 

*HDP Parameters*

| Parameter | Description |
|---|---|
| Stack | |
| Version | |
| OS | |
| Stack Repo Id | |
| Base Url | |
| Utils Repo Id | |
| Utils Base Url | |


#### Prewarmed Image

The **Prewarmed Image** option allows you to select a previously created custom image other than CentOS.


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