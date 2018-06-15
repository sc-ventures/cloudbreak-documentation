## Release notes

### 2.7.0

Cloudbreak 2.7.0 is a general availability release, which is suitable for production deployments.

____________________________

#### New features
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

#### New features (TP)

The following features are introduced in Cloudbreak 2.7 as technical preview; these features are for evaluation only and are not suitable for production.  
____________________________

##### **Data Lake Technical Preview**

Cloudbreak allows you to create a long-running data lake cluster and attach it to a short-running cluster. To get started, refer to [Setting up a data lake](data-lake.md).  
____________________________


##### **Gateway SSO Technical Preview**

As part of Apache Knox-powered gateway introduced in Cloudbreak 2.7, you can configure the gateway as the SSO identity provider. For more information, refer to [Configure single sign-on (SSO)](gateway.md#configure-single-sign-on-sso).  
____________________________

#### Behavioral changes
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

#### Image catalog updates
____________________________

Default versions provided with Cloudbreak 2.7:

Default Ambari version 2.6.2.0  
Default HDP version 2.6.5.0-292  
Default HDF version 3.1.1.0-35    

____________________________

#### Fixed issues
____________________________


| Issue | Issue description | Category | Fix version |
|---|---|---|---|
| BUG-101223 | After stopping and starting a cluster, cluster state is incorrectly listed as "Unhealty", even though the nodes are healthy. | Stability | 2.7.0 |
| BUG-101230 | The `curl` command listed on the *Download CLI* page for Windows does not work on Windows and therefore it should be removed or replaced. | Usability | 2.7.0 |
| BUG-101225 | `cb cluster repair` does not work as expected. | Usablity | 2.7.0 |
| BUG-101204 | Using the `instanceProfileStrategy` parameter in the CLI JSON from creating an instance profile does not work as expected.  | Usability | 2.7.0 |
| BUG-100468 | Images from a custom image catalog are not listed in the UI after Cloudbreak version changed. | Stability | 2.7.0 |
| BUG-99168 | All clusters created on Google Cloud Platform fail. | Stability | 2.5.0 TP |
| BUG-99400 | Time-based cluster autoscaling does not work. | Stability | 2.5.0 TP |
| BUG-99505 | Sync does not work for an AWS instance that was terminated a long time ago. | Stability | 2.5.0 TP |
| BUG-98277 | Network interface handling in CloudBreak should be improved. | Stability | 2.5.0 TP |
| BUG-97395 | Networks are duplicated on networks tab of the cluster create wizard. | Stability | 2.5.0 TP |
| BUG-97259 | "Update failed" status after downscale failed, even though cluster was not modified and its status should be "Running". | Stability | 2.5.0 TP |
| BUG-97207 | Changing lifecycle management on YARN causes NPE. | Stability | 2.5.0 TP |
| BUG-99189 | ImageCatalog PUT endpoint is not secured. | Security | 2.5.0 TP |
| BUG-97895 | LDAP password should be removed from Cloudbreak logs. | Security | 2.5.0 TP |
| BUG-97300 | Cloudbreak should show proper error messages when the given credential is not valid anymore. | Usability | 2.5.0 TP |
| BUG-97296| GCP credential creation should validate whether resources are available with the credential. | Usability | 2.5.0 TP |
| BUG-97660 | Ignore repository warnings checkbox are missing after changing base image Ambari or HDP to a custom one. | Usability | 2.5.0 TP |
| BUG-97307 | Ignore repository warnings checkbox is not selectable after change the HDP VDF URL. | Usability | 2.5.0 TP |
| BUG-96764 | "Failed to remove instance" error when using the delete icon. | Usability | 2.5.0 TP |
| BUG-97390 | Cloudbreak should support longer resource ID-s on AWS. | Usability | 2.5.0 TP |
| BUG-99512 | Azure ES_v3 instances should support premium storage. | Usability | 2.5.0 TP |
| BUG-97206 | Backend should return only images for enabled platforms. | Usability | 2.5.0 TP |

____________________________

#### Known issues
____________________________

**Known issues: Cloudbreak**
____________________________

##### (BUG-96788) **Azure Availability Set Option Is Not Available for Instance Count of 1**

When creating a cluster, the Azure availability set feature is not available for host groups with the instance count of 1.

*Workaround*:

If you would like to use the Azure availability sets feature now, you must add at least 2 instances to the host group for which you want to use them. The option Azure availability sets is available on the advanced **Hardware and Storage** page of the create cluster wizard.   
____________________________


##### (BUG-92605) **Cluster Creation Fails with ResourceInError**

Cluster creation fails with the following error:

*Infrastructure creation failed. Reason: Failed to create the stack for CloudContext{id=3689, name='test-exisitngnetwork', platform='StringType{value='OPENSTACK'}', owner='e0307f96-bd7d-4641-8c8f-b95f2667d9c6'} due to: Resource CREATE failed: ResourceInError: resources.ambari_volume_master_0_0: Went to status error due to "Unknown"*

*Workaround*:

This may mean that the volumes that you requested exceed volumes available on your cloud provider account. When creating a cluster, on the advanced **Hardware and Storage** page of the create cluster wizard, try reducing the amount of requested storage. If you need more storage, try using a different region or ask your cloud provider admin to increase the resource quota for volumes.  

[Comment]: <> (This jira item was closed so it will not be fixed. Maybe add this to troubleshooting?)
____________________________

##### (BUG-104825) **Upscaling the Compute Host Group is Not Possible on AWS**

When using Cloudbreak with Ambari 2.6.2, upscaling the compute host group on AWS fails with the following error due to Ambari being unable to install the HIVE_CLIENT:   

*New node(s) could not be added to the cluster. Reason com.sequenceiq.cloudbreak.core.ClusterException: Ambari operation failed: component: 'UPSCALE_REQUEST', requestID: '8'*

____________________________

##### (BUG-93241) **Error When Scaling Multiple Host Groups**

Scaling of multiple host groups fails with the following error:

*Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1; nested exception is org.hibernate.StaleStateException: Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1*

*Workaround:*

Scaling multiple host groups at once is not supported. If you would like to scale multiple host groups: scale the first host group and wait until scaling has completed, then scale the second host group, and so on.  
____________________________


##### (BUG-97044) **Show CLI Command Copy JSON Button Does Not Work**

When using the **Show CLI Command** > **Copy the JSON** or **Copy the Command** button with Firefox, the content does not does not get copied if adblock plugin or other advertise blocker plugins are present.

 *Workaround:*  

 Use a browser without an adblock plugin.

 [Comment]: <> (This jira item was closed.)

____________________________


##### (BUG-93257) **Clusters Are Missing From History When an Exact Day Is Selected**   

On the History page, when the start date selected is the same as end date, clusters that were running on that date are filtered out.

____________________________


##### (BUG-93257) **Clusters Are Missing From History**   

After changing the dates on the History page multiple times, the results displayed may sometimes be incorrect.

*Workaround:*

Refresh the page if you think that the history displayed may be incorrect.  

____________________________

**Known issues: Ambari and HDP**

The known issues described here were discovered when testing Cloudbreak with Ambari and HDP versions that are used by default in Cloudbreak. For general Ambari and HDP known issues, refer to Ambari and HDP release notes.   

____________________________


##### (BUG-96707) **Druid Overload Does Not Start**

Druid overload start fails with the following error when using Ambari 2.6.1.3 and HDP 2.6.4.0:

*ERROR [main] io.druid.cli.CliOverlord - Error when starting up.  Failing. com.google.inject.ProvisionException: Unable to provision*

____________________________

##### (BUG-97080) **Ambari Files In Some Cases When an Mpack is Installed**

If you set the following properties then cluster install may fail (in 20-30% of the cases), because of the Ambari agent cache being updated concurrently:

<pre>/etc/ambari-server/conf/ambari.properties  
agent.auto.cache.update=true*  
*/etc/ambari-agent/conf/ambari-agent.ini  
parallel_execution=1</pre>

[Comment]: <> (Fixed in Ambari 2.7?)
____________________________

##### (AMBARI-14149) **In Ambari, Cluster Cannot Be Started After Stop**

When using Ambari version 2.5.0.3, after stopping and starting a cluster, Event History shows the following error:

<pre>Ambari cluster could not be started. Reason: Failed to start Hadoop services.
2/7/2018, 12:47:05 PM
Starting Ambari services.
2/7/2018, 12:47:04 PM
Manual recovery is needed for the following failed nodes:   
[host-10-0-0-4.openstacklocal, host-10-0-0-3.openstacklocal, host-10-0-0-5.openstacklocal</pre>

Ambari dashboard shows that nodes are not sending heartbeats.

*Workaround:*  

This issue is fixed in Ambari version 2.5.1.0 and newer.  

[Comment]: <> (See BUG-96086, EAR-6780, AMBARI-14149)
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


##### (BUG-104109) **Websocket Connection fails on Ambari 2.7 Cluster** 

Websocket connection fails on Ambari 2.7 cluster with the following errors: 

*Uncaught RangeError: Maximum call stack size exceeded
    at RegExp.test (<anonymous>)
    at getPath (vendor.js:13470)
    at get (vendor.js:13353)
    at Class.get (vendor.js:19791)
    at Class.subscribe (app.js:197067)
    at Class.addHandler (app.js:197095)
    at Class.addHandler (app.js:197096)
    at Class.addHandler (app.js:197096)
    at Class.addHandler (app.js:197096)
    at Class.addHandler (app.js:197096)*
    
*Uncaught TypeError: Cannot read property 'handlers' of undefined
    at Class.removeHandler (app.js:197112)
    at Class.unsubscribeOfUpdates (app.js:21730)
    at Class.willDestroyElement (app.js:226835)
    at Class.newFunc [as willDestroyElement] (vendor.js:12954)
    at Class.trigger (vendor.js:25526)
    at Class.newFunc [as trigger] (vendor.js:12954)
    at Class.<anonymous> (vendor.js:25031)
    at Class.invokeRecursively (vendor.js:24941)
    at Class.<anonymous> (vendor.js:24944)
    at Class.forEachChildView (vendor.js:24727)*

    
[Comment]: <> (BUG 105295 needs more info)

    