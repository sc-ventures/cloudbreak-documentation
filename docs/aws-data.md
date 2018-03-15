## Access Data on S3  

Use these steps to configure access from your cluster to Amazon S3. 

### Prerequisites

To use S3 storage, you must have one or more S3 buckets on your AWS account. For instructions on how to create a bucket on S3, refer to [AWS documentation](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

**Related Links**  
[Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) (External)    


### Creating an IAM Role for S3 Access 

In order to configure access from your cluster to Amazon S3, you must have an existing IAM role which determines what actions can be performed on which S3 buckets. If you already have an IAM role, skip to the next step. If you do not have an existing IAM role, use the following instructions to create one. 

**Steps**

1. Navigate to the **IAM console** > **Roles** and click **Create Role**.

    <a href="../images/aws-s3-role_01.png" target="_blank" title="click to enlarge"><img src="../images/aws-s3-role_01.png" width="650" title="IAM Console"></a> 
    
2. In the "Create Role" wizard, select **AWS service** role type and then select **EC2** service and **EC2** use case. 

    <a href="../images/aws-s3-role_02.png" target="_blank" title="click to enlarge"><img src="../images/aws-s3-role_02.png" width="650" title="IAM Console"></a> 

3. When done, click **Next: Permissions** to navigate to the next page in the wizard.
    
4. Select an existing S3 access policy or click **Create policy** to define a new policy. If you are just getting started, you can select a built-in policy called "AmazonS3FullAccess", which provides full access to S3 buckets that are part of your account:

    <a href="../images/aws-s3-role_03.png" target="_blank" title="click to enlarge"><img src="../images/aws-s3-role_03.png" width="650" title="IAM Console"></a>
    
5. When done attaching the policy, click **Next: Review**.
    
6. In the **Roles name** field, enter a name for the role that you are creating:  

    <a href="../images/aws-s3-role_04.png" target="_blank" title="click to enlarge"><img src="../images/aws-s3-role_04.png" width="650" title="IAM Console"></a> 
    
7. Click **Create role** to finish the role creation process.


### Configuring Access to S3

Amazon S3 is not supported as a default file system, but access to data in S3 from your cluster VMs can be automatically configured by attaching an [instance profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) allowing access to S3. You can optionally create or attach an existing instance profile during cluster creation on the **File System** page.

To configure access to S3 with an instance profile, follow these steps. 

**Steps**

1. You or your AWS admin must create an IAM role with an S3 access policy which can be used by cluster instances to access one or more S3 buckets. Refer to [Creating an IAM Role for S3 Access](#creating-an-iam-role-for-s3-access).  
2. On the **File System** page in the advanced cluster wizard view, select **Use existing instance profile**. 
3. Select an existing IAM role created in step 1:

    <a href="../images/aws-s3-role_05.png" target="_blank" title="click to enlarge"><img src="../images/aws-s3-role_05.png" width="650" title="Cloudbreak UI"></a> 

During the cluster creation process, Cloudbreak assigns the IAM role and its associated permissions to the EC2 instances that are part of the cluster so that applications running on these instances can use the role to access S3.   


### Testing Access from HDP to S3

Amazon S3 is not supported in HDP as a default file system, but access to data in Amazon S3 is possible via the s3a connector. 

To tests access to S3 from HDP, SSH to a cluster node and run a few hadoop fs shell commands against your existing S3 bucket.

Amazon S3 access path syntax is:

<pre>s3a://bucket/dir/file</pre>

For example, to access a file called "mytestfile" in a directory called "mytestdir", which is stored in a bucket called "mytestbucket", the URL is:

<pre>s3a://mytestbucket/mytestdir/mytestfile</pre>

The following FileSystem shell commands demonstrate access to a bucket named "mytestbucket": 

<pre>hadoop fs -ls s3a://mytestbucket/

hadoop fs -mkdir s3a://mytestbucket/testDir

hadoop fs -put testFile s3a://mytestbucket/testFile

hadoop fs -cat s3a://mytestbucket/testFile
test file content</pre>

For more information about configuring the S3 connector for HDP and working with data stored on S3, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) documentation.

**Related Links**  
[Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) (Hortonworks)


