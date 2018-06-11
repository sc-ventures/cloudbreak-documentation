## Creating HDF clusters 

In general, creating HDF clusters is similar to creating to HHD clusters; however, there are a few HDF-specific configuration requirements that you should be aware of. 

### Creating HDF flow management clusters

When creating a Flow Management cluster from the default blueprint, make sure to do the following:

* Place the Ambari Server on the "Services" host group.     
* When creating a cluster, open 9091 TCP port on the NiFi host group. Without it, you will be unable to access the NiFi web UI.   
* When creating a cluster, open port 61443 on the Services host group. This port is used by NiFi Registry.      
* When creating the NiFi Registry controller service in NiFi, the internal hostname has to be used, `e.g. https://ip-1-2-3-4.us-west-2.compute.internal:61443`   
* Enable Kerberos. You can either use your own kerberos or select for Cloudbreak to create a test KDC.  
* Although Cloudbreak allows cluster scaling (including autoscaling), scaling is not supported by NiFi. Downscaling NiFi clusters is not supported - as it can result in data loss when a node is removed that has not yet processed all the data on that node. There is also a known issue related to scaling listed in the [Known Issues](#known-issues) below.  

For the list of available blueprints, refer to [Default Cluster Configurations](index.html#default-cluster-configurations).  
To get started creating NiFi clusters, refer to the following [HCC post](https://community.hortonworks.com/articles/182221/create-a-nifi-cluster-on-aws-azure-google-or-opens.html). 

### Creating HDF messaging management clusters

When creating a Messaging Management cluster from the default blueprint, make sure to do the following:

* If using the default blueprint, place the Ambari Server on the "Services" host group.  
* When creating a cluster, open 3000 TCP port on the Services host group for Grafana.    


