## Accessing Data on Azure 

HDP 2.6 supports reading and writing both block blobs and page blobs from/to Windows Azure Storage Blob (WASB) object store, as well as reading and writing files stored in an Azure Data Lake Storage (ADLS) account. This allows you to:

* Persist data using cloud storage services beyond the lifetime of your HDP clusters.  
* Load data in Hadoop ecosystem applications directly from Azure storage services, without first importing or uploading data from external resources to HDFS.  
* Use other applications (not necessarily in your Hadoop ecosystem) to manipulate the data stored in Azure storage services beyond the lifetime of your HDP clusters.  
* Share data between multiple HDP clusters fast and easily by pointing to the same Azure data sets. 
* Move or copy data between different Azure storage services or between Azure storage services and HDFS to facilitate different scenarios for big data analytics workloads.  
* Back up unlimited archive data at any scale from HDP cluster to fully managed, durable, and highly available Azure storage services.   


### Accessing Data in ADLS

[Azure Data Lake Store (ADLS)](https://azure.microsoft.com/en-in/services/data-lake-store/) is an enterprise-wide hyper-scale repository for big data analytic workloads.

#### Prerequisites

If you want to use [Azure Data Lake Store](https://azure.microsoft.com/en-in/services/data-lake-store/) to store your data, you must: 

1. Enable Azure subscription for Data Lake Store, and then create an Azure Data Lake Store [storage account](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal) in the same subscription as you use for launching Cloudbreak controller and clusters.  
 
2. Next, in the cluster creation phase, specify the created account name only, and access to the Azure Data Lake Store will be configured automatically. For further instructions, refer to Microsoft Azure [documentation](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal).

#### Configuring Access to ADLS 

ADLS is not supported as a default file system, but access to data in ADLS is automatically configured if you select ADLS during [cluster creation](azure-create.md), on the **Add File System** page.

#### Access Path

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


### Accessing Data in WASB  

Windows Azure Storage Blob (WASB) is an object store service available on Azure.

#### Prerequisites

If you want to use Windows Azure Storage Blob to store your data, you must enable Azure subscription for Blob Storage, and then create a [storage account](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account#create-a-storage-account).  

#### Configuring Access to WASB 

In order to access data stored in your Azure blob storage account, you must configure your storage account access key in `core-site.xml`. The configuration property that you must use is `fs.azure.account.key.<account name>.blob.core.windows.net` and the value is the access key. 

For example the following property should be used for a storage account called "testaccount": 

```
<property>
  <name>fs.azure.account.key.testaccount.blob.core.windows.net</name>
  <value>TESTACCOUNT-ACCESS-KEY</value>
</property>
```

You can obtain your access key from the Access keys in your storage account settings.

Alternatively, it is possible to configure `fs.defaultFS` to use a wasb or wasbs URL. This causes all bare paths, such as /testDir/testFile to resolve automatically to that file system.

#### Access Path

WASB access path syntax is:

<pre>wasb://<b>container_name</b>@<b>storage_account_name</b>.blob.core.windows.net/<b>dir/file</b></pre>

For example, to access a file called "testfile" located in a directory called "testdir", stored in the container called "testcontainer" on the account called "hortonworks", the URL is:

<pre>wasb://testcontainer@hortonworks.blob.core.windows.net/testdir/testfile</pre>

You can also use "wasbs" prefix to utilize SSL-encrypted HTTPS access:

<pre>wasbs://<container_name>@<storage_account_name>.blob.core.windows.net/dir/file</pre>

### Learn More

For more information about configuring the ADLS and WASB connectors and working with data stored in ADLS and WASB, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_cloud-data-access/content/about.html) documentation.

