## Accessing data on Azure 

Hortonworks Data Platform (HDP) supports reading and writing both block blobs and page blobs
from/to *Windows Azure Storage Blob (WASB)* object store, as well as reading and writing files stored in an
*Azure Data Lake Storage (ADLS)* account. 

### Accessing data in ADLS from HDP 

[Azure Data Lake Store (ADLS)](https://azure.microsoft.com/en-in/services/data-lake-store/) is an enterprise-wide hyper-scale repository for big data analytic workloads.

#### Prerequisites

If you want to use ADLS to store your data, you must enable Azure subscription for Data Lake Store, and then create an Azure Data Lake Store [storage account](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal).

#### Configure access to ADLS 

ADLS is not supported as a default file system, but access to data in ADLS via the adl connector. To configure access to ADLS from a cluster managed via Cloudbreak use the steps described in [How to configure authentication with ADLS](https://community.hortonworks.com/articles/105994/how-to-configure-authentication-with-adls.html).

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
    

#### Working with ADLS  

For more information about configuring the ADLS connector and working with data stored in ADLS, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) documentation.

**Related links**   
[Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) (Hortonworks)  
[How to configure authentication with ADLS](https://community.hortonworks.com/articles/105994/how-to-configure-authentication-with-adls.html) (Hortonworks)  
[Azure Data Lake Store](https://azure.microsoft.com/en-in/services/data-lake-store/) (External)     
[Create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account#create-a-storage-account) (External)     
[Get started with Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal) (External)  




### Accessing data in WASB from HDP 

Windows Azure Storage Blob (WASB) is an object store service available on Azure.

#### Prerequisites

If you want to use Windows Azure Storage Blob to store your data, you must enable Azure subscription for Blob Storage, and then create a [storage account](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account#create-a-storage-account).  

#### Configure access to WASB 

In order to access data stored in your Azure blob storage account, you must configure your storage account access key in `core-site.xml`. The configuration property that you must use is `fs.azure.account.key.<account name>.blob.core.windows.net` and the value is the access key. 

For example the following property should be used for a storage account called "testaccount": 

```
<property>
  <name>fs.azure.account.key.testaccount.blob.core.windows.net</name>
  <value>TESTACCOUNT-ACCESS-KEY</value>
</property>
```

You can obtain your access key from the Access keys in your storage account settings.


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


#### Working with WASB  

For more information about configuring the WASB connector and working with data stored in WASB, refer to [Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) documentation.

**Related links**   
[Cloud Data Access](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.2/bk_cloud-data-access/content/about.html) (Hortonworks)   
[Create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account#create-a-storage-account) (External)   

