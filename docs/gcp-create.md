## Creating a Cluster on GCP 

{!docs/common/create-a1.md!}
| Region | Select the GCP region in which you would like to launch your cluster. For information on available GCP regions, refer to [GCP documentation](https://cloud.google.com/about/locations/). |
{!docs/common/create-a2.md!}
| Instance Type | Select a VM instance type. For information about instance types on GCP refer to [GCP documentation](https://cloud.google.com/compute/docs/machine-types).|
{!docs/common/create-a3.md!}
| SSH Public Key | Specify a public SSH key. You will use the matching private key to access your cluster nodes via SSH. |

{!docs/common/create-a4.md!}

**Related Links**  
[Blueprints](blueprints.md)   
[Default Cluster Security Groups](security.md#default-cluster-security-groups)   
[CIDR](http://www.ipaddressguide.com/cidr) (External)   
[Cloud Locations](https://cloud.google.com/about/locations/) (External)  
[Machine Types](https://cloud.google.com/compute/docs/machine-types) (External)     


### Advanced Options

{!docs/common/create-adv-1.md!}


#### Availability Zone

 Choose one of the availability zones within the selected region. 
 
 
{!docs/common/create-adv-2.md!}
| Storage Type |  <p>Select the volume type. The options are:<ul><li>Standard persistent disks (HDD)</li><li>Solid-state persistent disks (SSD)</li></ul> For more information about these options refer to <a href="https://cloud.google.com/compute/docs/disks/" target="_blank">GCP documentation</a>. |
{!docs/common/create-adv-3.md!}

**Related Links**  
[Storage Options](https://cloud.google.com/compute/docs/disks/) (External)   

#### Use Preemptible Instances

Check this option to use Google Cloud preemptive VM instances as your cluster nodes. To learn more, refer to [Google Cloud documentation](https://cloud.google.com/compute/docs/instances/preemptible).    

Note that: 

* We recommend not using preemptible instances for any host group that includes Ambari server components.  
* If you choose to use preemptible instances for a given host group when creating your cluster, any nodes that you add to that host group (during cluster creation or later) will be using preemptible instances.   
* If you decide not to use preemptible instances when creating your cluster, any nodes that you add to your host group (during cluster creation or later) will be using standard on-demand instances.     
* Once someone outbids you, the preemptible instances are taken away, removing the nodes from the cluster. 
* If the preemptible instances are not available right away, creating a cluster will take longer than usual. 

[commemt]: <> (There is no bid specified in the UI, so I assume that we are using current bid?)

**Related Links**   
[Preemptible VM Instances](https://cloud.google.com/compute/docs/instances/preemptible) (External)    


{!docs/common/create-adv-4.md!} 

{!docs/common/create-adv-5.md!}


<div class="next">
<a href="../gcp-clusters-access/index.html">Next: Access Cluster</a>
</div>


