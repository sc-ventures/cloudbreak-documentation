## Creating a cluster on GCP 

{!docs/common/create-a1.md!}
| Region | Select the GCP region in which you would like to launch your cluster. For information on available GCP regions, refer to [GCP documentation](https://cloud.google.com/about/locations/). |
{!docs/common/create-a2.md!}
| Instance Type | Select a VM instance type. For information about instance types on GCP refer to [GCP documentation](https://cloud.google.com/compute/docs/machine-types).|
{!docs/common/create-a3-0.md!}
| Storage Type |  <p>Select the volume type. The options are:<ul><li>Standard persistent disks (HDD)</li><li>Solid-state persistent disks (SSD)</li></ul> For more information about these options refer to <a href="https://cloud.google.com/compute/docs/disks/" target="_blank">GCP documentation</a>. |
{!docs/common/create-a3-1a.md!}
{!docs/common/create-a3-1b.md!}
{!docs/common/create-a3-2.md!}

    > Existing security groups are only available for an existing network. 
    
{!docs/common/create-a4.md!}
{!docs/common/create-a5.md!}

**Related Links**  
[Flex support subscription](get-help.md#flex-subscription)  
[Using custom blueprints](blueprints.md)   
[Default cluster security groups](security.md#default-cluster-security-groups)  
[Troubleshooting cluster creation](trouble-cluster.md)      
[CIDR](http://www.ipaddressguide.com/cidr) (External)   
[Cloud locations](https://cloud.google.com/about/locations/) (External)  
[Machine types](https://cloud.google.com/compute/docs/machine-types) (External)     


### Advanced options

{!docs/common/create-adv-1.md!}


#### Availability zone

 Choose one of the availability zones within the selected region. 
 
 
{!docs/common/create-adv-2.md!}


####  Root volume size 

Use this option to increase the root volume size. Default is 50 GB. This option is useful if your custom image requires more space than the default 50 GB. 


#### Use preemptible instances

Check this option to use Google Cloud preemptive VM instances as your cluster nodes. To learn more, refer to [Google Cloud documentation](https://cloud.google.com/compute/docs/instances/preemptible).    

Note that: 

* We recommend not using preemptible instances for any host group that includes Ambari server components.  
* If you choose to use preemptible instances for a given host group when creating your cluster, any nodes that you add to that host group (during cluster creation or later) will be using preemptible instances.   
* If you decide not to use preemptible instances when creating your cluster, any nodes that you add to your host group (during cluster creation or later) will be using standard on-demand instances.     
* Once someone outbids you, the preemptible instances are taken away, removing the nodes from the cluster. 
* If the preemptible instances are not available right away, creating a cluster will take longer than usual. 

[Comment]: <> (There is no bid specified in the UI, so I assume that we are using current bid?)


#### Cloud storage 

If you would like to access Google Cloud Storage (GCS) from your cluster, you must configure access as described in [Accessing data on GCS](gcp-data.md). 

**Related links**  
[Accessing data on GCS](gcp-data.md)     


{!docs/common/create-adv-4.md!} 

{!docs/common/create-adv-5.md!}

{!docs/common/create-adv-6.md!}  
[Preemptible VM instances](https://cloud.google.com/compute/docs/instances/preemptible) (External)   
[Storage options](https://cloud.google.com/compute/docs/disks/) (External)  



<div class="next">
<a href="../gcp-data/index.html">Next: Configure Access to GCS</a>
</div>


