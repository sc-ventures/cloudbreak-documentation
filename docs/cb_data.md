## Configuring cloud data access 


## Configuring access to Amazon S3  

Amazon S3 is not supported as a default file system, but access to data in S3 is possible via the s3a connector. Use these steps to configure access from your cluster to Amazon S3. 

These steps assume that you are using an HDP version that supports the s3a cloud storage connector (HDP 2.6.1 or newer).  

### Creating an IAM role for S3 access 

In order to configure access from your cluster to Amazon S3, you must have an existing IAM role which determines what actions can be performed on which S3 buckets. If you already have an IAM role, skip to the next step. If you do not have an existing IAM role, use the following instructions to create one. 

**Steps**

1. Navigate to the **IAM console** > **Roles** and click **Create Role**.

    <img src="../images/cb_aws-s3-role_01.png" width="650" title="IAM Console"> 
    
2. In the "Create Role" wizard, select **AWS service** role type and then select **EC2** service and **EC2** use case. 

    <img src="../images/cb_aws-s3-role_02.png" width="650" title="IAM Console"> 

3. When done, click **Next: Permissions** to navigate to the next page in the wizard.
    
4. Select an existing S3 access policy or click **Create policy** to define a new policy. If you are just getting started, you can select a built-in policy called "AmazonS3FullAccess", which provides full access to S3 buckets that are part of your account:

    <img src="../images/cb_aws-s3-role_03.png" width="650" title="IAM Console">
    
5. When done attaching the policy, click **Next: Review**.
    
6. In the **Roles name** field, enter a name for the role that you are creating:  

    <img src="../images/cb_aws-s3-role_04.png" width="650" title="IAM Console">
    
7. Click **Create role** to finish the role creation process.


### Configure access to S3  

Amazon S3 is not supported as a default file system, but access to data in S3 from your cluster VMs can be automatically configured by attaching an [instance profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) allowing access to S3. You can optionally create or attach an existing instance profile during cluster creation on the **Cloud Storage** page.

To configure access to S3 with an instance profile, follow these steps. 

**Steps**

1. You or your AWS admin must create an IAM role with an S3 access policy which can be used by cluster instances to access one or more S3 buckets. Refer to [Creating an IAM role for S3 access](https://hortonworks.github.io/cloudbreak-documentation/latest/).    
2. On the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing instance profile**.  
3. Select an existing IAM role created in step 1:

During the cluster creation process, Cloudbreak assigns the IAM role and its associated permissions to the EC2 instances that are part of the cluster so that applications running on these instances can use the role to access S3.   


### Testing access from HDP to S3

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

For more information about configuring the S3 connector for HDP and working with data stored on S3, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_cloud-data-access/content/about.html) documentation.


### Configure S3 storage locations

After configuring access to S3 via instance profile, you can optionally use an S3 bucket as a base storage location; this storage location is mainly for the Hive Warehouse Directory (used for storing the table data for managed tables).   

**Prerequisites** 

* You must have an existing bucket. For instructions on how to create a bucket on S3, refer to [AWS documentation](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).   
* The instance profile that you configured under [Configure access to S3](https://hortonworks.github.io/cloudbreak-documentation/latest/) must allow access to the bucket.   

**Steps**

1. When creating a cluster, on the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing instance profile** and select the instance profile to use, as described in [Configure access to S3](https://hortonworks.github.io/cloudbreak-documentation/latest/).    
2. Under **Storage Locations**, enable **Configure Storage Locations** by clicking the <img src="../images/cb_toggle.png" alt="On"/> button.   
3. Provide your existing bucket name under **Base Storage Location**. 
    
[Comment]: <> (I tried setting this to some bucket which did not exist and the location was not set. Can we check if the bucket exists? Or state in the UI that this has to be an existing location?)

[Comment]: <> (Does the "/apps/hive/warehouse" directory structure get created automatically? Or do I have to create it? I tried setting this and hive.metastore.warehouse.dir was set as expected but no directories were created within my bucket. Is that expected?)
    
4. Under **Path for Hive Warehouse Directory property (hive.metastore.warehouse.dir)**, Cloudbreak automatically suggests a location within the bucket. For example, if the bucket that you specified is `my-test-bucket` then the suggested location will be `my-test-bucket/apps/hive/warehouse`.  You may optionally update this path.   

    Cloudbreak automatically creates this directory structure in your bucket.      

[Comment]: <> (What is the difference between disabling by using the toggle button and choosing "Do not configure"?)

## Configuring access to ADLS  

Hortonworks Data Platform (HDP) supports reading and writing block blobs and page blobs from and to *Windows Azure Storage Blob (WASB)* object store, as well as reading and writing files stored in an *Azure Data Lake Storage (ADLS)* account. 

[Azure Data Lake Store (ADLS)](https://azure.microsoft.com/en-in/services/data-lake-store/) is an enterprise-wide hyper-scale repository for big data analytic workloads. ADLS is not supported as a default file system, but access to data in ADLS is possible via the adl connector. 

These steps assume that you are using an HDP version that supports the adl cloud storage connector (HDP 2.6.1 or newer).  


#### Prerequisites

If you would like to use ADLS to store your data, you must enable Azure subscription for Data Lake Store, and then create an Azure Data Lake Store [storage account](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal).


#### Configure access to ADLS 

To configure authentication with ADLS using the client credential, you must register a new application with Active Directory service and then give your application access to your ADL account. After you've performed these steps, you can provide your application's information when creating a cluster. 

**Steps**

1. Register an application and add it to the ADLS account, as described in *Step 1* and *Step 2* of [How to configure authentication with ADLS](https://community.hortonworks.com/articles/105994/how-to-configure-authentication-with-adls.html). 

    > Do not perform the *Step 3* described in this article. Cloudbreak automates this step.  
    
2. In Cloudbreak web UI, on the advanced **Cloud Storage** page of the create a cluster wizard, select **Use existing ADLS storage**.

3. Provide the following parameters for your registered application:    

    * **ADLS Account Name**: This is the ADL account that your application was assigned to.    
    * **Application ID**: You can find it in your application's settings. 
    * **Key**: This is the key that you generated for your application. If you did not copy the it, you must create a new key from the Keys page in your application's settings.   

Once your cluster is in the running state, you will be able to access the Azure blob storage account from the cluster nodes. 


#### Test access to ADLS

To tests access to ADLS, SSH to a cluster node and run a few hadoop fs shell commands against your existing ADLS account.

ADLS access path syntax is:

<pre>adl://<b>account_name</b>.azuredatalakestore.net/<b>dir/file</b></pre>

For example, the following Hadoop FileSystem shell commands demonstrate access to a storage account named "myaccount":

<pre>hadoop fs -mkdir adl://myaccount.azuredatalakestore.net/testdir</pre>

<pre>hadoop fs -put testfile adl://myaccount.azuredatalakestore.net/testdir/testfile</pre>

To use DistCp against ADLS, use the following syntax:
<pre>hadoop distcp
    [-D hadoop.security.credential.provider.path=localjceks://file/home/user/adls.jceks]
    hdfs://<b>namenode_hostname</b>:9001/user/foo/007020615
    adl://<b>myaccount</b>.azuredatalakestore.net/testDir/</pre>
    

For more information about configuring the ADLS connector and working with data stored in ADLS, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_cloud-data-access/content/about.html) documentation.



#### Configure ADLS storage locations

After configuring access to ADLS, you can optionally use that ADLS storage account as a base storage location; this storage location is mainly for the Hive Warehouse Directory (used for storing the table data for managed tables).   

**Steps**

1. When creating a cluster, on the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing ADLS storage** and select the instance profile to use, as described in [Configure access to ADLS](#configure-access-to-adls).    
2. Under **Storage Locations**, enable **Configure Storage Locations** by clicking the <img src="../images/cb_toggle.png" alt="On"/> button.   
3. Provide your existing directory name under **Base Storage Location**. 

    > Make sure that the directory exists within the account.
    
[Comment]: <> (Is this correct that this needs to be a directory name and that it needs to exist already?)    
    
4. Under **Path for Hive Warehouse Directory property (hive.metastore.warehouse.dir)**, Cloudbreak automatically suggests a location within the bucket. For example, if the directory that you specified is `my-test-dir` then the suggested location will be `my-test-adls-account.azuredatalakestore.net/my-test-dir/apps/hive/warehouse`. You may optionally update this path.        

    Cloudbreak automatically creates this directory structure in your bucket.  
    
## Configuring access to WASB

Hortonworks Data Platform (HDP) supports reading and writing block blobs and page blobs from and to *Windows Azure Storage Blob (WASB)* object store, as well as reading and writing files stored in an *Azure Data Lake Storage (ADLS)* account.  

Windows Azure Storage Blob (WASB) is an object store service available on Azure. WASB is not supported as a default file system, but access to data in WASB is possible via the wasb connector.

These steps assume that you are using an HDP version that supports the wasb cloud storage connector (HDP 2.6.1 or newer).  


#### Prerequisites

If you want to use Windows Azure Storage Blob to store your data, you must enable Azure subscription for Blob Storage, and then create a [storage account](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account#create-a-storage-account).

Here is an example of how to create a a storage account:
    
<img src="../images/cb_wasb1.png" width="650" title="WASB">


#### Configure access to WASB 

In order to access data stored in your Azure blob storage account, you must obtain your access key from your storage account settings, and then provide the storage account name and its corresponding access key when creating a cluster.     

**Steps**

1. Obtain your storage account name and storage access key from the **Access keys** page in your storage account settings:
 
    <img src="../images/cb_wasb2.png" width="650" title="WASB">
 
2. In Cloudbreak web UI, on the advanced **Cloud Storage** page of the create a cluster wizard,  select **Use existing WASB storage**.  

3. Provide the *Storage Account Name* and *Access Key* parameters obtained in the previous step.   

Once your cluster is in the running state, you will be able to access the Azure blob storage account from the cluster nodes. 


#### Test access to WASB

To tests access to WASB, SSH to a cluster node and run a few hadoop fs shell commands against your existing WASB account.

WASB access path syntax is:

<pre>wasb://<b>container_name</b>@<b>storage_account_name</b>.blob.core.windows.net/<b>dir/file</b></pre>

For example, to access a file called "testfile" located in a directory called "testdir", stored in the container called "testcontainer" on the account called "hortonworks", the URL is:

<pre>wasb://testcontainer@hortonworks.blob.core.windows.net/testdir/testfile</pre>

You can also use "wasbs" prefix to utilize SSL-encrypted HTTPS access:

<pre>wasbs://<container_name>@<storage_account_name>.blob.core.windows.net/dir/file</pre>

The following Hadoop FileSystem shell commands demonstrate access to a storage account named "myaccount" and a container named "mycontainer":

<pre>hadoop fs -ls wasb://mycontainer@myaccount.blob.core.windows.net/

hadoop fs -mkdir wasb://mycontainer@myaccount.blob.core.windows.net/testDir

hadoop fs -put testFile wasb://mycontainer@myaccount.blob.core.windows.net/testDir/testFile

hadoop fs -cat wasb://mycontainer@myaccount.blob.core.windows.net/testDir/testFile
test file content</pre>
 

For more information about configuring the WASB connector and working with data stored in WASB, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_cloud-data-access/content/about.html) documentation.
  


#### Configure WASB storage locations

After configuring access to WASB, you can optionally use that WASB storage account as a base storage location; this storage location is mainly for the Hive Warehouse Directory (used for storing the table data for managed tables).   

**Steps**

1. When creating a cluster, on the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing WASB storage** and select the instance profile to use, as described in [Configure access to WASB](#configure-access-to-wasb).    
2. Under **Storage Locations**, enable **Configure Storage Locations** by clicking the <img src="../images/cb_toggle.png" alt="On"/> button.   
3. Provide your existing directory name under **Base Storage Location**. 

    > Make sure that the container exists within the account.
    
[Comment]: <> (Is this correct that this needs to be the container name and that the container must exist already?)    
    
4. Under **Path for Hive Warehouse Directory property (hive.metastore.warehouse.dir)**, Cloudbreak automatically suggests a location within the bucket. For example, if the directory that you specified is `my-test-container` then the suggested location will be `my-test-container@my-wasb-account.blob.core.windows.net/apps/hive/warehouse`. You may optionally update this path.  

    Cloudbreak automatically creates this directory structure in your bucket.  
    
## Configuring access to Google Cloud Storage (GCS)

Google Cloud Storage (GCS) is not supported as a default file system, but access to data in GCS is possible via the gs connector. Use these steps to configure access from your cluster to GCS. 

These steps assume that you are using an HDP version that supports the gs cloud storage connector (HDP 2.6.5 introduced this feature as a TP).

### Prerequisites

Access to Google Cloud Storage is via a service account. The service account that you provide to Cloudbreak for GCS data access must have the following permissions: 

* The service account must have the project-wide **Owner** role:

    <img src="../images/cb_gcp-data-sa01.png" width="550" title="GCP Console">  

* The service account must have the **Storage Object Admin** role for the bucket that you are planning to use. You can set this in the bucket's permissions settings:

    <img src="../images/cb_gcp-data-sa02.png" width="450" title="GCP Console">      

### Configure access to GCS 

Access to Google Cloud Storage is via a service account. 

**Steps**

1. On the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing GSC storage**. 

2. Under **Service Account Email Address** provide the email for your GCS storage account.  

Once your cluster is in the running state, you should be able to access buckets that the configured storage account has access to.  


### Testing access to GCS 

Test access to the Google Cloud Storage bucket by running a few commands from any cluster node. For example, you can use the command listed below (replace “mytestbucket” with the name of your bucket):

<pre>hadoop fs -ls gs://my-test-bucket/</pre>


### Configure GCS storage locations 

After configuring access to GCS via service account, you can optionally use a GCS bucket as a base storage location; this storage location is mainly for the Hive Warehouse Directory (used for storing the table data for managed tables).   

**Prerequisites**

* You must have an existing bucket. For instructions on how to create a bucket on GCS, refer to [GCP documentation](https://cloud.google.com/storage/docs/creating-buckets).  
* The service account that you configured under [Configure access to GCS](#configure-access-to-gcs) must allow access to the bucket.   

**Steps**

1. When creating a cluster, on the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing GCS storage** and select the existing service account, as described in [Configure access to GCS](#configure-access-to-gcs).    
2. Under **Storage Locations**, enable **Configure Storage Locations** by clicking the <img src="../images/cb_toggle.png" alt="On"/> button.   
3. Provide your existing bucket name under **Base Storage Location**.  
4. Under **Path for Hive Warehouse Directory property (hive.metastore.warehouse.dir)**, Cloudbreak automatically suggests a location within the bucket. For example, if the bucket that you specified is `my-test-bucket` then the suggested location will be `my-test-bucket/apps/hive/warehouse`.  You may optionally update this path.        

    Cloudbreak automatically creates this directory structure in your bucket.  
    
