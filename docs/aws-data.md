## Access Data on S3  

#### Prerequisites

To use S3 storage, you must have one or more S3 buckets on your AWS account. For instructions on how to create a bucket on S3, refer to [AWS documentation](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

**Related Links**  
[Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) (External)    


#### Configuring Access to S3

Amazon S3 is not supported as a default file system, but access to data in Amazon S3 is possible via the s3a connector. 

Data in public S3 buckets can be accessed without any access configuration. 

During cluster creation, Cloudbreak creates a new IAM role "S3AccessRole" to grant the cluster instances access to S3 buckets within your AWS account. The  "S3AccessRole" IAM role policy is as follows: 

<pre>{
    "Statement": [
        {
            "Action": "s3:*",
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}</pre>


[comment]: <> (Commenting out the following since this option is not available in 2.1 TP)

[comment]: <> (Amazon S3 is not supported as a default file system, but access to data in S3 from your cluster VMs can be automatically configured through attaching an instance profile allowing access to S3. You can optionally create or attach an existing instance profile during [cluster creation](aws-create.md#choose-blueprint), on the **File System** page.) 


#### Testing Access to S3

To tests access to S3, SSH to a cluster node and run a few hadoop fs shell commands against your existing S3 bucket.

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


### Learn More

For more information about configuring the S3 connector and working with data stored on S3, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) documentation.

**Related Links**  
[Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) (Hortonworks)


