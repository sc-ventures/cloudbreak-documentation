## Create a Cluster on AWS 

{!docs/common/create-a1.md!}
| Region | Select the AWS region in which you would like to launch your cluster. For information on available AWS regions, refer to [AWS documentation](http://docs.aws.amazon.com/general/latest/gr/rande.html). |
{!docs/common/create-a2.md!}
| Instance Type | Select an instance type. For information about instance types on AWS refer to [AWS documentation](https://aws.amazon.com/ec2/instance-types/). |
{!docs/common/create-a3.md!}
| SSH Key Pair| Select an existing public key. You will use the matching private key to access your cluster nodes via SSH. |
| Specify new SSH public key | Check this option to specify a new public key and then enter the public key. You will use the matching private key to access your cluster nodes via SSH. |

{!docs/common/create-a4.md!}

**Related Links**  
[Blueprints](blueprints.md)   
[Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) (External)   
[AWS Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) (External)     
[CIDR](http://www.ipaddressguide.com/cidr) (External)   



### Advanced Options

{!docs/common/create-adv-1.md!}


#### Availability Zone

 Choose one of the availability zones within the selected region. 
 
{!docs/common/create-adv-2.md!}
| Storage Type | <p>Select the volume type. The options are:<ul><li>Magnetic (default)</li><li>General Purpose (SSD)</li><li>Throughput Optimized HDD</li></ul>For more information about these options refer to <a href="http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html" target="_blank">AWS documentation</a>.</p>|
{!docs/common/create-adv-3.md!}

**Related Links**  
[Amazon EC2 Instance Store](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) (External)  


{!docs/common/create-adv-4.md!}

{!docs/common/create-adv-5.md!}


<div class="next">
<a href="../aws-clusters-access/index.html">Next: Access Cluster</a>
</div>
