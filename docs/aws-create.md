## Creating a Cluster on AWS 

{!docs/common/create-a1.md!}
| Region | Select the AWS region in which you would like to launch your cluster. For information on available AWS regions, refer to [AWS documentation](http://docs.aws.amazon.com/general/latest/gr/rande.html). |
{!docs/common/create-a2.md!}
| Instance Type | Select an instance type. For information about instance types on AWS refer to [AWS documentation](https://aws.amazon.com/ec2/instance-types/). |
{!docs/common/create-a3-1.md!}
    > On AWS, you can configure it to use private IPs instead. For instructions, refer to [Configure Communication via Private IPs](trouble-cluster.html#configure-communication-via-private-ips-aws). 

{!docs/common/create-a3-2.md!}

    > Existing security groups are only available for an existing VPC. 

{!docs/common/create-a4.md!}
{!docs/common/create-a5.md!}

**Related Links**  
[Blueprints](blueprints.md)   
[Default Cluster Security Groups](security.md#default-cluster-security-groups)  
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


#### Use Spot Instances

Check this option to use EC2 spot instances as your cluster nodes. Next, enter your bid price. The price that is pre-loaded in the form is the current on-demand price for your chosen EC2 instance type.   


Note that: 

* We recommend not using spot instances for any host group that includes Ambari server components.  
* If you choose to use spot instances for a given host group when creating your cluster, any nodes that you add to that host group (during cluster creation or later) will be using spot instances. Any additional nodes will be requested at the same bid price that you entered when creating a cluster.  
* If you decide not to use spot instances when creating your cluster, any nodes that you add to your host group (during cluster creation or later) will be using standard on-demand instances.     
* Once someone outbids you, the spot instances are taken away, removing the nodes from the cluster. 
* If spot instances are not available right away, creating a cluster will take longer than usual. 

After creating a cluster, you can view your spot instance requests, including bid price, on the EC2 dashboard under **INSTANCES** > **Spot Requests**. For more information about spot instances, refer to [AWS documentation](https://aws.amazon.com/ec2/spot/).  


#### File System 

HDP uses HDFS as the default filesystem and it supports accessing the Amazon S3 object store through the S3A connector. 

If you would like to access S3 through the S3A connector, you must configure access to S3 trough an instance profile. For instructions, refer to [Configuring Access to S3](aws-data.md#configuring-access-to-s3). 


{!docs/common/create-adv-4.md!}

{!docs/common/create-adv-5.md!}

{!docs/common/create-adv-6.md!}  
[Amazon EC2 Instance Store](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) (External)  
[Amazon EC2 Spot Instances](https://aws.amazon.com/ec2/spot/) (External)   


<div class="next">
<a href="../aws-clusters-access/index.html">Next: Access Cluster</a>
</div>
