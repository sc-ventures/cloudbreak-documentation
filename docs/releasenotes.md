## Release Notes


### 2.1.0 TP 

This release is technical preview: it is not suitable for production environments.

#### New Features

##### New UI/UX

Cloudbreak 2.1.0 TP introduces a new user interface.

##### New CLI

Cloudbreak 2.1.0 TP introduces a new CLI tool. For more information, refer to the [Install CLI](cli-install.md) and [CLI Reference](cli-reference.md) documentation. 


#### Behavioral Changes

##### Creating Custom Images 

The functionality which enables you to create custom images was changed and improved. Refer to  [Custom Images](images.md).

##### Removal of Cloudbreak Shell 

Cloudbreak Shell is no longer available in Cloudbreak 2.1.0 TP and later. It was replaced by the [Cloudbreak CLI](cli-install.md).

##### Removal of Platforms 

The [platforms](http://hortonworks.github.io/cloudbreak-docs/release-1.16.4/topologies/) feature was removed. 

##### Removal of Mesos 

Cloudbreak 2.1.0 TP does not support Mesos cloud provider.

##### Removal of Templates

Earlier versions of Cloudbreak allowed you to save infrastructure, network, and security group templates. This feature was removed. Instead, you can define VMs, storage, networks, and security groups as part of the create cluster wizard. 


#### Known Issues

##### Auto-scaling Is Not Available

Auto-scaling functionality is not available in Cloudbreak 2.1.0 TP. 


