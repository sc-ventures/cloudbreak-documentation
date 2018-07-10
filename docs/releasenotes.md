## Release Notes

### 2.7.1

Cloudbreak 2.7.1 is a general availability release, which is suitable for production deployments.

____________________________

#### New Features
____________________________

##### **New Prewarmed Images**

Coudbreak 2.7.1 includes new prewarmed images with Ambari 2.6.2.2. For more information, refer to [Image Catalog Updates](#image-catalog-updates).  
____________________________

##### **Launching Cloudbreak from Templates**

Cloudbreak 2.7.0 introduces a new way to launch Cloudbreak from cloud provider templates on AWS and Google Cloud; On Azure, this option was previously available. These are quickstart options, not suitable for production. To launch Cloudbreak by using these quickstart options refer to [Quickstart on AWS](aws-quick.md), [Quickstart on Azure](azure-quick.md), and [Quickstart on GCP](gcp-quick.md). To review current Cloudbreak deployment options (quickstart and production), refer to [Deployment options](deployment-options.md).    
____________________________

##### **Protected Gateway Powered by Apache Knox**

To access HDP cluster resources, a gateway powered by Apache Knox is configured. When creating a cluster, you can optionally instruct Cloudbreak to install and configure this gateway to protect access to the cluster resources. By default, transport layer security on the gateway endpoint is via a self-signed SSL certificate on port 8443. By default, Ambari is proxied through the gateway. For more information, refer to [Configuring a Gateway](gateway.md).  
____________________________

##### **Microsoft Azure ADLS and WASB Cloud Storage**

When creating a cluster on Azure, you can configure access to ADLS and WASB from the *Cloud Storage* page of the advanced create cluster wizard. For more information, refer to [Access data in ADLS](azure-data-adls.md) and [Access data in WASB](azure-data-wasb.md).  
____________________________

##### **Google GCS Cloud Storage**

When creating a cluster on Google Cloud, you can configure access to Google Cloud Storage from the *Cloud Storage* page of the advanced create cluster wizard. Authentication with GCS is via a service account. For more information, refer to [Access data in GCS](gcp-data.md).    
____________________________

##### **Base Cloud Storage Locations**

After configuring access to S3, ADLS, WASB, or GCS you can optionally use these as a base storage location, primarily for the Hive Warehouse Directory. For more information, refer to Configure ADLS storage locations on [Amazon S3](aws-data.md#configure-s3-storage-locations), [ADLS](azure-data-adls.md#configure-adls-storage-locations), [WASB](azure-data-wasb.md#configure-wasb-storage-locations), [GCS](gcp-data.md#configure-gcs-storage-locations).  
____________________________

##### **Custom Properties**

Cloudbreak allows you to add custom property variables (by using mustache template syntax) in your blueprint for replacement, and then set custom properties on a per-cluster basis. For more information, refer to [Custom properties](properties.md).  
____________________________

##### **Dynamic Blueprints**

Cloudbreak allows you to create special "dynamic" blueprints which include templating: the values of the variables specified in the blueprint are dynamically replaced in the cluster creation phase, picking up the parameter values that you provided in the Cloudbreak UI or CLI.
Dynamic blueprints offer the ability to manage external sources (such as RDBMS and LDAP/AD) outside of your blueprint.  
For more information, refer to [Dynamic Blueprints](concepts.md#dynamic-blueprints) and [Creating dynamic blueprints](blueprints.md#creating-dynamic-blueprints).   
____________________________

##### **HDF Clusters**

Cloudbreak introduces the ability to create HDF flow management clusters with Apache NiFi and NiFi Registry, as well as HDF messaging clusters with Apache Kafka. To help you get started, Cloudbreak provides two new built-in blueprints:  

* Flow Management: Apache NiFi  
* HDF Messaging Management: Apache Kafka   

For more information, refer to [Creating HDF clusters](hdf.md).   
____________________________

##### **External Databases for Cluster Services**

You can register an existing external RDBMS in the Cloudbreak UI or CLI so that it can be used for those cluster components which have support for it. After the RDBMS has been registered with Cloudbreak, it will be available during the cluster create and can be reused with multiple clusters. For more information, refer to [Register an External Database](external-db.md).  
____________________________

##### **External Authentication Source (LDAP/AD) for Clusters**

You can configure an existing LDAP/AD authentication source in the Cloudbreak UI or CLI so that it can later be associated with one or more Cloudbreak-managed clusters. After the authentication source has been registered with Cloudbreak, it will be available during the cluster create and can be reused with multiple clusters. For more information, refer to [Register an Authentication Source](external-ldap.md).    
____________________________  

##### **Using an Existing LDAP/AD for Cloudbreak**

You can configure Cloudbreak to use your existing LDAP/AD so that you can authenticate Cloudbreak users against an existing LDAP/AD server. For more information, refer to [Configuring Cloudbreak for LDAP/AD Authentication](cb-ldap.md).  
____________________________

##### **Launching Cloudbreak in Environments with Restricted Internet Access or Required Use of Proxy**

You can launch Cloudbreak in environments with limited or restricted internet access and/or required use of a proxy to obtain internet access. For more information, refer to [Configure Outbound Internet Access and Proxy](cb-proxy.md).  
____________________________

##### **Management Packs**

Cloudbreak 2.7 introduces support for using management packs, allowing you to register them in Cloudbreak web UI or CLI and then select to install them as part of cluster creation. For more information, refer to [Using management packs](mpacks.md).   
____________________________

##### **Modifying Existing Cloudbreak Credentials**

Cloudbreak allows you to modify existing credentials by using the edit option available in Cloudbreak UI or by using the `credential modify` command in the CLI. For more information, refer to [Modify an Existing Credential](cb-credentials.md#modify-an-existing-credential).  
____________________________

##### **Retrying Failed Clusters**

When stack provisioning or cluster creation failure occurs, the new "retry" UI option allows you to resume the process from the last failed step. A corresponding `cb cluster retry` CLI command has been introduced. For more information, refer to [Retry a cluster](clusters-manage.md#retry-a-cluster) and [CLI](cli-reference.md) documentation.  
____________________________

##### **Root Volume Size Configuration**

When creating a cluster, you can modify the root volume size. This option is available on the advanced **Hardware and Storage** page of the create cluster wizard. Default is 50 GB for AWS and GCP, and 30 GB for Azure. This option is useful if your custom image requires more space than provided by default.   
____________________________

##### **JSON Key on Google Cloud**

Cloudbreak introduces support for Google Cloud's service account JSON key. Since activating service accounts with P12 private keys has been deprecated in the Cloud SDK, we recommend using JSON private keys. For updated instructions for creating a Cloudbreak credential, refer to [Create Cloudbreak credential](gcp-launch.md#create-cloudbreak-credential).   
____________________________

##### **Viewing Cluster Blueprints**

Cloudbreak includes a useful option to view blueprints of a future cluster (from the create cluster wizard) or an existing cluster (from cluster details). For more information, refer to [View cluster blueprints](clusters-access.md#view-cluster-blueprints).   
____________________________

##### **CLI Autocomplete**

Cloudbreak CLI now includes an autocomplete option. For more information, refer to [Configure CLI autocomplete](cli-install.md#configure-cli-autocomplete).   
____________________________

##### **Instructions for Using Custom Hostnames on AWS** 

New documentation is available for using custom hostnames based on DNS for clusters running on AWS. For instructions, refer to [Using custom hostnames based on DNS](hostnames.md).  
____________________________

#### New Features (TP)

The following features are introduced in Cloudbreak 2.7 as technical preview; these features are for evaluation only and are not suitable for production.  
____________________________

##### **Data Lake Technical Preview**

Cloudbreak allows you to create a long-running data lake cluster and attach it to a short-running cluster. To get started, refer to [Setting up a data lake](data-lake.md).  
____________________________


##### **Gateway SSO Technical Preview**

As part of Apache Knox-powered gateway introduced in Cloudbreak 2.7, you can configure the gateway as the SSO identity provider. For more information, refer to [Configure single sign-on (SSO)](gateway.md#configure-single-sign-on-sso).  
____________________________

#### Behavioral Changes
____________________________

##### **Removal of Prebuilt Cloudbreak Deployer Images**

In earlier versions of Cloudbreak, Cloudbreak deployer images for AWS, Google Cloud, and OpenStack were provided for each release. Instead, Cloudbreak 2.7.0 introduces a new way to launch Cloudbreak from cloud provider templates. To review current Cloudbreak deployment options, refer to [Deployment options](deployment-options.md).   
____________________________ 

##### **Auto-import of HDP/HDF Images on OpenStack**

When using Cloudbreak on OpenStack, you no longer need to import HDP and HDF images manually, because during your first attempt to create a cluster, Cloudbreak automatically imports HDP and HDF images to your OpenStack. Only Cloudbreak image must be imported manually.
____________________________

##### **Installing Mysql Connector as a Recipe**

Starting with Ambari version 2.6, if you have 'MYSQL_SERVER' component in your blueprint, you have to [manually install and register](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.2.0/bk_ambari-administration/content/using_hive_with_mysql.html) the 'mysql-connector-java.jar'. If you would like to automate this process in Cloudbreak, you can apply the [following recipe](recipes.md#install-mysql-connector-recipe).   
____________________________

##### **Sorting and Filtering Resource Tables in the UI**

Cloudbreak introduces the ability to sort and filter resource tables that list resources such as clusters, cluster hardware, blueprints, recipes, and so on, in the Cloudbreak web UI.  
____________________________

##### **Redesigned Hardware and Storage UI**

The UI of the **Hardware and Storage** page in the create cluster wizard and in cluster details was redesigned for better user experience.   
____________________________

##### **New Image Settings Page in Create Cluster wizard**

All cluster options related to image settings, image catalog selection, and Ambari and HDP/HDF repository specification were moved to a separate **Image Settings** page in the create cluster wizard.     
____________________________

##### **File System Cluster Page Was Renamed to Cloud Storage**

The **File System** page of the advanced create cluster wizard was renamed to **Cloud Storage**.     
____________________________

##### **Image Catalog Option Was Moved to External Sources**

The options related to registering a custom image catalog and selecting a default image catalog were removed from the **Settings** navigation menu option and are now available under **External Sources > Image Catalogs**.  
____________________________

##### **Recipes Option Was Moved to Cluster Extension**

The **Recipes** navigation menu option was moved under **Cluster Extensions**, so to find recipe-related settings, select **Cluster Extensions > Recipes** from the navigation menu.  
____________________________

#### Image Catalog Updates
____________________________

Update for Cloudbreak 2.7.1:

Default Ambari version 2.6.2.2  
Default HDP version 2.6.5.0-292  
Default HDF version 3.1.2.0  

Default versions provided with Cloudbreak 2.7:

Default Ambari version 2.6.2.0  
Default HDP version 2.6.5.0-292  
Default HDF version 3.1.1.0-35  

____________________________

#### Fixed Issues
____________________________

| Issue | Issue description | Category | Fix Version |
|---|---|---|---|
| BUG-106925 | Log requests to AWS to circumvent throttling error. | Usability | 2.7.1 |
| BUG-106665 | Exception if availability-zone is not set on AWS. | Usability | 2.7.1 |
| RMP-11639 | Support python shebangs in recipe. | Usability | 2.7.1 |
| BUG-105586 | Sync error when you delete an instance on provider. | Usability | 2.7.1 |
| BUG-105901 | Improve ImageCatalogService logging. | Usability | 2.7.1 |
| BUG-106143 | Mpack request can be duplicated during cluster creation. | Usability | 2.7.1 |
| BUG-106229 | Custom blueprint inputs feature is not working form the web UI. | Usability | 2.7.1 |
| BUG-106419 | In the web UI > create cluster > Time to to Live, the input text says that TTL is in minutes, but the hint text says that it is in milliseconds. | Usability | 2.7.1 |
| BUG-106440, BUG-106346 | Unable to add RDS in the UI when a name contains hyphen. | Usability | 2.7.1 |
| BUG-106554 | Cloudbreak credentials input should not allow browser caching of data. | Security | 2.7.1 |
| BUG-106742 | LLAP tests can fail, because they are started with 1 worker node. | Stability | 2.7.1 |
| BUG-106776 | On AWS, under create cluster > Storage, the maximum disk size greater than allowed by AWS. | Usability | 2.7.1 |
| BUG-106779 | In the Cloudbreak web UI > create cluster on AWS, disk min size does not change when disk type is changed. | Usability | 2.7.1 |
| BUG-104393 | Check conditional handlebar templates for accidental invalid templates. | Stability | 2.7.1 |
| BUG-105288 | Display expected decommission duration on Cloudbreak. | Usability | 2.7.1 |
| BUG-105976 | Configure network interface in salt grains runtime instead of image burning. | Stability | 2.7.1 |
| BUG-105997 | m5 instance type on AWS should allow for ST1 storage. | Usability | 2.7.1 |
| BUG-106158 | Wrong blueprint is generated if locations has duplicated key. | Usability | 2.7.1 |
| BUG-106331 | Remove StructuredFlowEventFactory read only transaction. | Stability | 2.7.1 |
| BUG-106369 | Add Atlas AD authentication configs. | Stability | 2.7.1 |
| BUG-106480	| JSON template generated for a running attached cluster does not include "sharedService". | Usability | 2.7.1 |
| BUG-106534, BUG-106464 | Incorrect AWS instance profile is selected for a cluster. | Usability | 2.7.1 |
| BUG-106645 | GCP cloud storage connector requires project-id in core-site. | Stability | 2.7.1 |
| BUG-105065 | Error message is missing when registering an LDAP server with no port via CLI. | Usability | 2.7.1 |
| BUG-101226 | Scaling a cluster when a node is in CREATED state causes unpredictable results. | Usability | 2.7.1 |
| BUG-105188 | On OpenStack, cluster creation hangs with "Stack has flows under operation. | Stability | 2.7.1 |
| BUG-96788  | Azure Availability Set option is not available for instance count of 1. | Usability | 2.7.1 |
| BUG-105440 | LDAPS does not work. | Stability | 2.7.0 |
| BUG-105191 | LLAP is enabled with EDW-ETL blueprint. | Stability | 2.7.0 |
| BUG-105061 | NullPointerException when kerberized cluster is being terminated. | Stability | 2.7.0 |
| BUG-105057	| Cloudbreak recipe in "pre-termination" stage does not run till completion. Machine shuts down in between. | Stability | 2.7.0 |
| BUG-104950 | cbd update causes data loss. | Stability | 2.7.0 | 2.7.0 |
| BUG-104949 | NullPointerException during upscale. | Stability | 2.7.0 |
| BUG-104948 | Cannot delete instance when the upscale failed. | Stability | 2.7.0 |
| BUG-104947 | Cluster status should be updated to AVAILABLE even when there are stopped services. | Stability | 2.7.0 |
| BUG-104931 | When it aborts scaling, Cloudbreak should return a message showing which services are stopped. | Stability | 2.7.0 |
| BUG-104930 | Upscale needs to be limited to 100 instances per one request. | Stability | 2.7.0 |
| BUG-104915 | Cluster termination failed when kerberos is enabled. | Stability | 2.7.0 |
| BUG-104889 | NullPointerException during stack creation. | Stability | 2.7.0 |
| BUG-104790 | NullPointerException when scaling up an HDF cluster to 1000 nodes on OpenStack. | Stability | 2.7.0 |
| BUG-104789 | Some error messages in the CLI and UI are hard to understand. | Usability | 2.7.0 |
| BUG-104787 | UI menu is not scrollable (unusable in case of small window). | Usability | 2.7.0 |
| BUG-104786 | "Copy JSON" button text is invalid | Usability | 2.7.0 |
| BUG-104785 | In the UI, the sync option is disabled but clickable (on a stopped cluster). | Usability | 2.7.0 |
| BUG-104782 | Wrong region is selected after cluster creation. | Stability | 2.7.0 |
| BUG-104779 | EDW-Analytics blueprint fails on AWS. | Stability | 2.7.0 |
| BUG-104759 | Cannot load custom image catalog. | Usability | 2.7.0 |
| BUG-104758 | Credential error causes invalid error message: "Failed to VM types for the cloud provider". | Usability | 2.7.0 |
| BUG-104544 | In create stack request, improve support for deprecated gateway requests. | Stability | 2.7.0 |
| BUG-104530 | The host groups in the validation [Services,ZooKeeper,NiFi] must match the host groups in the request [Services,NiFi] | Stability | 2.7.0 |
| BUG-104529 | Request to acquire token failed. | Stability | 2.7.0 |
| BUG-104480 | JPA has too many connections. | Stability | 2.7.0 |
| BUG-104475 | Unable to acquire JDBC connection. | Stability | 2.7.0 |
| BUG-104473 | Missing node configuration in the blueprint causes NullPointerException. | Stability | 2.7.0 |
| BUG-104469 | Failed to get platform networks java.lang.IllegalArgumentException: No region provided. | Stability | 2.7.0 |
| BUG-104451 | Could not verify credential [credential: 'temp-user-credential'], detailed message: Unauthorized. | Stability | 2.7.0 |
| BUG-104450 | Failed to get platform networks com.google.api.client.googleapis.json.GoogleJsonResponseException. | Stability |  2.7.0 |
| BUG-104445	|  Error during stack termination flow: java.lang.NullPointerException. | Stability | 2.7.0 |
| BUG-104275 | Structured events contain passwords and sensitive data. | Security | 2.7.0 |
| BUG-104274 | HDP cluster version is incorrect in structured events. | Stability | 2.7.0 |
| BUG-104235 | Filter valid images by provider | Usability | 2.7.0 |
| BUG-104124 | Registering Postgres RDS causes Null Pointer Exception. | Stability | 2.7.0 |
| BUG-104120 | Back-and-forth navigation in the create cluster wizard confuses the UI and opens the wrong port (443 instead of 8443). | Usability | 2.7.0 |
| BUG-103678 | Remove instanceProfileStrategy recommendation from the CLI. | Stability | 2.7.0 |
| BUG-102890 | Periscope result returns more than one elements for cluster. | Stability | 2.7.0 |
| BUG-102884 | Saved instance groups will be removed if credential has changed. | Usability | 2.7.0 |
| BUG-102732 | CLI can't reinstall a cluster without the `--blueprint-name` flag. | Usability | 2.7.0 |
| BUG-102730 | Null Pointer Exception during stack repair. | Stability | 2.7.0 |
| BUG-102714 | Cannot update the status of cluster 'X' to STARTED, because the stack is not in AVAILABLE state. | Stability | 2.7.0 |
| BUG-102711 | Null Pointer Exception during downscale if Ambari is not reachable. | Stability | 2.7.0 |
| BUG-102441 | Can't create Azure cluster after wrong ssh-rsa key was submitted. | Stability | 2.7.0 |
| BUG-102296 | Openstack4j glance V2 error. | Stability | 2.7.0 |
| BUG-102201 | Images are shown for regions that have no available images. | Usability | 2.7.0 |
| BUG-101988 | Match tag restrictions with cloud provider's restrictions. | Usability | 2.7.0 |
| BUG-101746 | Duplicated error message when backend is down. | Usability | 2.7.0 |
| BUG-101483 | Sometimes the hostnames are not good after cluster installation. | Stability | 2.7.0 |
| BUG-101473 | Cloudbreak CLI doesn't show any error if the cloudbreak host is wrong. | Stability | 2.7.0 |
| BUG-101236 | Time to live not calculated or displayed well. | Usability | 2.7.0 |
| BUG-101231 | After Recipe delete: page not found. | Usability | 2.7.0 |
| BUG-101230 | The `curl` command listed on the *Download CLI* page for Windows does not work on Windows and therefore it should be removed or replaced. | Usability | 2.7.0 |
| BUG-101228 | History for the same start and end date filters out cluster. | Usability | 2.7.0 |
| BUG-101225 | CLI `cb cluster repair` does not work as expected. | Usability | 2.7.0 | 
| BUG-101223 | After stopping and starting a cluster, cluster state is incorrectly listed as "Unhealty", even though the nodes are healthy. | Stability | 2.7.0 |
| BUG-101222 | Filter by button should be removed from UI External sources > Authentication configs. | Usability | 2.7.0 |
| BUG-101204 | Using the `instanceProfileStrategy` parameter in the CLI JSON from creating an instance profile does not work as expected.  | Usability | 2.7.0 |
| BUG-100844 | In the Cloudbreak UI create cluster wizard the side menu is incorrect for Cluster Extensions. | Usability | 2.7.0 |
| BUG-100684	| Zeppelin Shiro config is wrong. | Stability | 2.7.0 |
| BUG-100549 | GCP cluster creation failed if existing subnet is defined. | Stability | 2.7.0 |
| BUG-100468 | Images from a custom image catalog are not listed in the UI after Cloudbreak version changed. | Stability | 2.7.0 |
| BUG-100110 | Availability zones do not refreshed in Cloudbreak to match the actual AWS availability zones. | Usability | 2.7.0 |
| BUG-100027 | Change default instance/storage settings in AWS Paris region. | Usability | 2.7.0 |
| BUG-99168 | All clusters created on Google Cloud Platform fail. | Stability |  2.7.0 |
| BUG-99400 | Time-based cluster autoscaling does not work. | Stability | 2.7.0 |
| BUG-99505 | Sync does not work for an AWS instance that was terminated a long time ago. | Stability | 2.7.0 |
| BUG-98277 | Network interface handling in CloudBreak should be improved. | Stability | 2.7.0 |
| BUG-97395 | Networks are duplicated on networks tab of the cluster create wizard. | Stability | 2.7.0 |
| BUG-97259 | "Update failed" status after downscale failed, even though cluster was not modified and its status should be "Running". | Stability | 2.7.0 |
| BUG-97207 | Changing lifecycle management on YARN causes NPE. | Stability | 2.7.0 |
| BUG-99189 | ImageCatalog PUT endpoint is not secured. | Security | 2.7.0 |
| BUG-97895 | LDAP password should be removed from Cloudbreak logs. | Security | 2.7.0 | 
| BUG-97300 | Cloudbreak should show proper error messages when the given credential is not valid anymore. | Usability | 2.7.0 |
| BUG-97296| GCP credential creation should validate whether resources are available with the credential. | Usability | 2.7.0 |
| BUG-97660 | Ignore repository warnings checkbox are missing after changing base image Ambari or HDP to a custom one. | Usability | 2.7.0 |
| BUG-97307 | Ignore repository warnings checkbox is not selectable after change the HDP VDF URL. | Usability | 2.7.0 |
| BUG-96764 | "Failed to remove instance" error when using the delete icon. | Usability | 2.7.0 |
| BUG-97390 | Cloudbreak should support longer resource ID-s on AWS. | Usability | 2.7.0 |
| BUG-99512 | Azure ES_v3 instances should support premium storage. | Usability | 2.7.0 |
| BUG-97206 | Backend should return only images for enabled platforms. | Usability | 2.7.0 |

____________________________

#### Known Issues
____________________________

**Known issues: Cloudbreak**
____________________________

##### (BUG-92605) **Cluster Creation Fails with ResourceInError**

Cluster creation fails with the following error:

*Infrastructure creation failed. Reason: Failed to create the stack for CloudContext{id=3689, name='test-exisitngnetwork', platform='StringType{value='OPENSTACK'}', owner='e0307f96-bd7d-4641-8c8f-b95f2667d9c6'} due to: Resource CREATE failed: ResourceInError: resources.ambari_volume_master_0_0: Went to status error due to "Unknown"*

*Workaround*:

This may mean that the volumes that you requested exceed volumes available on your cloud provider account. When creating a cluster, on the advanced **Hardware and Storage** page of the create cluster wizard, try reducing the amount of requested storage. If you need more storage, try using a different region or ask your cloud provider admin to increase the resource quota for volumes.  

[Comment]: <> (This jira item was closed so it will not be fixed. Maybe add this to troubleshooting?)

____________________________

##### (BUG-105439) **When the Master Node Goes Down the Cluster Status Is Available**

When the master node goes down or is removed, the cluster remains in available status. 

*Workaround:*
To obtain the actual node status, check the **Cluster History** and the **Hardware** section. If any cluster nodes are removed, they will be listed in red in under **Hardware** and there should be an event logged in the **Cluster History**. 
____________________________

##### (BUG-105312) **Wrong Notification After Deleting an Autoscaling Policy**

When you delete an existing scaling policy or an existing alert, you will see the following confirmation message: "Scaling policy / Alert has been saved". This message should state: "Scaling policy / Alert has been deleted". The deletion occurs correctly, but the confirmation message is incorrect.  

*Workaround*:  
Check the UI to ensure that the deletion took place. Deletion occurs correctly, but the confirmation message is incorrect.  
____________________________

##### (BUG-105205) **Auto Repair Does Not Work with a HA Cluster**

When the ambari server host was removed from a HA cluster with autorepair on ambari server hostgroup, no autorepair occurred.BUG-105308
____________________________

##### (BUG-105308) **Exception When a Pending Cluster is Terminated**

In some cases, when a cluster is terminated while its creation process is still pending, the cluster termination fails with the following exception:  

*Unable to find com.sequenceiq.cloudbreak.domain.Constraint with id 2250* 

*Workaround:*  
Try terminating the cluster again and selecting the force terminate option. 
____________________________

##### (BUG-105309) **Error When Deleting a Custom Blueprint**

In some cases, when a cluster is terminated while its creation process is still pending, blueprint remains attached to the cluster, causing the following error when one tries to delete the blueprint: 

*There is a cluster 'perftest-d0e5q5y7lj' which uses blueprint 'multinode-hdfs-yarn-ez0plywf2c'. Please remove this cluster before deleting the blueprint*

*Workaround:*  
Log in to the Cloudbreak database and try deleting the bleuprrint manually. 

____________________________

##### (BUG-105090) **Unable to Edit Optional Credential Parameters**

When modifying an existing CLoudbreak credential, it is possible to modify mandatory parameters, but it is not possible to save changes in the optional parameters due to the *Save* button remaining disabled.

*Workaround*:  
You have two options:
- Use the CLI to modify the credential. This is the only option if you have running clusters that use the credential.   
- If you do not have any running clusters, delete the credential and create a new one.  
____________________________

##### (BUG-97044) **Show CLI Command Copy JSON Button Does Not Work**

When using the *Show CLI Command* > *Copy the JSON* or *Copy the Command* button with Firefox, the content does not does not get copied if adblock plugin or other advertise blocker plugins are present.

*Workaround:*   
Use a browser without an adblock plugin.

[Comment]: <> (This jira item was closed.)
____________________________

**Known issues: Ambari**

The known issues described here were discovered when testing Cloudbreak with Ambari versions that are used by default in Cloudbreak. For general Ambari and known issues, refer to Ambari release notes.  
____________________________

There are no known issues related to Ambari.  
____________________________

**Known issues: HDP**

The known issues described here were discovered when testing Cloudbreak with HDP versions that are used by default in Cloudbreak. For general HDP known issues, refer to HDP release notes.  
____________________________

There are no known issues related to HDP.  
____________________________

**Known issues: HDF**

The known issues described here were discovered when testing Cloudbreak with  the hDF version used by default in Cloudbreak. For general HDF known issues, refer to HDF release notes.  
____________________________

##### (BUG-98865) **Scaling HDF Clusters Does Not Update Configurations on New Nodes**

Blueprint configuration parameters are not applied when scaling an HDF cluster.
One example that affects all users is that after HDF cluster upscale/downscale the `nifi.web.proxy.host` blueprint parameter does not get updated to include the new nodes, and as a result the NiFi UI is not reachable from these nodes.

*Workaround:*  

Configuration parameters set in the blueprint are not applied when scaling an HDF cluster. One example that affects all NiFi users is that after HDF cluster upscale the `nifi.web.proxy.host` parameter does not get updated to include the new hosts, and as a result the NiFi UI is not reachable from these hosts.

`HOST1-IP:PORT,HOST2-IP:PORT,HOST3-IP:PORT`
____________________________

[Comment]: <> (For 2.7.0: BUG-105307 is not clear, did not add it)
   
