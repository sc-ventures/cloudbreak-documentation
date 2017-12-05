## Create a Cluster on OpenStack 

{!docs/common/create-a1.md!}
| Region | Select the region in which you would like to launch your cluster. |
{!docs/common/create-a2.md!}
| Instance Type | Select an instance type. |
{!docs/common/create-a3.md!}
| SSH Public Key | Select an existing public key or specify a new public key. You will use the matching private key to access your cluster nodes via SSH. |

{!docs/common/create-a4.md!}

**Related Links**  
[Blueprints](blueprints.md)   
[Default Cluster Security Groups](security.md#default-cluster-security-groups)  
[CIDR](https://www.ipaddressguide.com/cidr) (External)    
    

### Advanced Options

{!docs/common/create-adv-1.md!}


#### Availability Zone

 Choose one of the availability zones within the selected region. 
 

{!docs/common/create-adv-2.md!}
| Storage Type | <p>Select the volume type. The options are:<ul><li>Magnetic</li><li>Ephemeral</li><li>General Purpose (SSD)</li><li>Throughput Optimized HDD</li></ul>For more information about these options refer to <a href="http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html" target="_blank">AWS documentation</a>.</p>|
{!docs/common/create-adv-3.md!}


{!docs/common/create-adv-4.md!}

{!docs/common/create-adv-5.md!}


<div class="next">
<a href="../os-clusters-access/index.html">Next: Access Cluster</a>
</div>


