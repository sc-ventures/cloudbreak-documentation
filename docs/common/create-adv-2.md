#### Custom Images

[Comment]: <> (TO DO: Not sure if the UI is final? Not sure if I understand the feature?)

By default, Cloudbreak uses a CentOS image and which HDP and Ambari versions??

**Base Image** option: Allows you to customize Ambari/HDP version while using the default CentOS image.

**Prewarmed Image** option: Allows you to select a previously created custom image other than CentOS.

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