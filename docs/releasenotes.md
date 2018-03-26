## Release Notes

### 2.4.0

Cloudbreak 2.4.0 is a general availability release, which is suitable for production deployments. 



____________________________

#### New Features
____________________________



##### New UI/UX

Cloudbreak 2.4.0 introduces a new user interface. 

All major options are now easily accessible from the collapsible navigation menu. All UI options and wizards have been redesigned in order to make cluster creation and management more intuitive. 

The following screenshot shows the cluster dashboard: 

<a href="../images/cb-ui3.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui3.png" width="650" title="Cloudbreak web UI"></a> 

The following screenshot shows cluster details page:
 
> Note that autoscaling is available from the **Autoscaling** tab.  

> Note the **Show CLI Command** available from **Actions** menu. This allows you to generate a CLI template of the cluster. A similar option is available on the last page of the create cluster wizard.   

<a href="../images/cb-ui4.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui4.png" width="650" title="Cloudbreak web UI"></a> 

[Comment]: <> (Adding this screenshot, which shows autoscaling, because it seems like everyone is thinking that autoscaling is no longer available in Cloudbreak.)

The following screenshot shows the create cluster wizard:

> Note the **Basic/Advanced** toggle button, which allows you to switch between the basic and advanced wizard view. In the **Settings** you can select which view you would like to see by default.   

<a href="../images/cb-ui5.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui5.png" width="650" title="Cloudbreak web UI"></a> 

[Comment]: <> (Adding this screenshot, because many SEs have missed the Basic/Advanced toggle button.)

____________________________


##### New CLI

Cloudbreak 2.4.0 introduces the new CLI tool, which replaces Cloudbreak Shell. All commands start with a singular object followed by an action, for example, `blueprint create`  and `blueprint list`. To download the CLI, select **Download CLI** from the navigation pane. Refer to [Install CLI](cli-install.md), [Get Started with the CLI](cli-get-started.md), and [CLI Reference](cli-reference.md).  
____________________________


##### Support for Kerberos

Creating Kerberos-enabled clusters is supported:  

- Use an existing MIT KDC or Active Directory for production deployments.  
- Use a test KDC for evaluation purposes.  

Refer to [Enabling Kerberos Security](security-kerberos.md).
____________________________


##### Support for Configuring an External RDBMS for Cloudbreak

Using an external RDBMS for Cloudbreak is supported and recommended for production environments.   
For configuration instructions, refer to [Configuring External Cloudbreak Database](cb-db.md).
____________________________


##### Support for Migrating Cloudbreak Instance

Migrating Cloudbreak from one machine to another is supported.  
For migration instructions, refer to [Moving a Cloudbreak Instance](cb-migrate.md).
____________________________


##### Prewarmed Images 

To accelerate cluster creation, CLoudbreak 2.4.0 introduces prewarmed images, which include the operating system, as well as the default version of Ambari and HDP. By default, Cloudbreak 2.4 launches clusters from these prewarmed images, instead of using base images (which were used by default in earlier versions of Cloudbreak). Default base images are still available in case you would like to use different Ambari and HDP versions than those provided with prewarmed images.  
For more information, refer to [Prewarmed and Base Images](aws-create.md#prewarmed-and-base-images). 
____________________________


##### Providing Your Own JDK

Providing your own JDK on a custom base image is supported. For instructions, refer to "Advanced topics" in the [https://github.com/hortonworks/cloudbreak-images](https://github.com/hortonworks/cloudbreak-images) repository.
____________________________


##### Generating CLI Templates 

After specifying the parameters for your cluster in the Cloudbreak web UI, you can copy the content of the CLI JSON file that can be used to create a cluster via Cloudbreak CLI. For more information, refer to [Obtain Cluster JSON Template from the UI](cli-get-started.md#obtain-cluster-json-template-from-the-ui).    

Furthermore, Cloudbreak web UI includes an option in the UI which allows you to generate the  `create` command for resources such as credentials, blueprints, clusters, and recipes. For more information, refer to [Obtain CLI Command from the UI](cli-get-started.md#obtain-cli-command-from-the-ui).  
____________________________


##### New Recipe Types 

New types of recipes are introduced:

* PRE-AMBARI-START (new, useful for configuring Ambari prior to start)  
* POST-AMBARI-START (formerly known as PRE)  
* POST-CLUSTER-INSTALL (formerly known as POST)  
* PRE-TERMINATION (new, useful for cluster cleanup pre-termination tasks)  

Refer to updated [Recipes](recipes.md) documentation.
____________________________


##### Disabling Cloud Providers 

You can hide cloud providers available in Cloudbreak by adding the `CB_ENABLEDPLATFORMS` environment variable in Profile and setting it to the provider(s) that you would like to have available. For more information, refer to [Disable Providers](cb-disable-provider.md).
____________________________


##### Support for Ambari 2.6 (2.6.1.3+)

Cloudbreak 2.4.0 introduces support for Ambari 2.6 and uses Ambari 2.6.1.3 by default. If you would like to use Ambari 2.6, you must use Ambari version 2.6.1.3 or newer. 

If you decide to use Ambari 2.6.1, you should be aware of the following:

- Ambari 2.6.1 or newer does not install the mysqlconnector; therefore, when creating a blueprint for Ambari 2.6.1 or newer do not include the MYSQL_SERVER component for Hive Metastore in your blueprint. Instead, you have two options described in [Creating a Blueprint](blueprints.md#creating-a-blueprint).  
- If you would like to use Oozie, you must manually install Ext JS. The steps are described in [Cannot Access Oozie Web UI](trouble-cluster.md#cannot-access-oozie-web-ui).   
- To enable LZO compression in your HDP cluster, you must check the "Enable Ambari Server to download and install GPL Licensed LZO packages?" during cluster creation. The option is available under **Security** > **Prewarmed and Base Images**.  

[Comment]: <> (Or should these be described as behavioral changes?) 
____________________________


##### Ambari Master Key 

Cloudbreak 2.4.0 allows you to specify the Ambari Master Key. The Ambari Server Master Key is used to configure Ambari to encrypt database and Kerberos credentials that are retained by Ambari as part of the Ambari setup. The option is available on the **Security** page of the create cluster wizard.







____________________________

#### Behavioral Changes
____________________________

##### Custom Images

The functionality which enables you to create custom images was changed and improved. Refer to [Custom Images](images.md).
____________________________


##### Prewarmed Images Are Used By Default  

By default, Cloudbreak 2.4.0 launches clusters from prewarmed images, instead of base images (which were used by default in earlier versions of Cloudbreak). For more information, refer to [Prewardmed and Base Images](aws-create.md#prewarmed-and-base-images). 
____________________________


##### Removal of Cloudbreak Shell

Cloudbreak Shell is not available in Cloudbreak 2.x. It was replaced by the [Cloudbreak CLI](cli-install.md).
____________________________


##### Removal of Platforms

The [Platforms](http://hortonworks.github.io/cloudbreak-docs/release-1.16.5/topologies/) feature was removed and is not available in Cloudbreak 2.x.
____________________________


##### Removal of Mesos

Mesos cloud provider is not supported and the corresponding options were removed in Cloudbreak 2.x.
____________________________


##### Removal of Templates

Earlier versions of Cloudbreak allowed you to save infrastructure, network, and security group templates. This feature was removed. Instead, you can define VMs, storage, networks, and security groups as part of the create cluster wizard.
____________________________


##### cbd update Improvements 

The following improvements were added for `cbd update`:

* Cloudbreak performs an automatic backup to prevent accidental data loss.   
* cbd delete requires confirmation.  
* Cloudbreak stores `psql` command history on the data volume to help investigate support cases.    
* There are two new files in the cbd deployment directory: .cbd_history and .cbd_output_history to help investigate support cases.   
* The default 10 second stop timeout is increased to 60 seconds. This can be overridden by setting DOCKER_STOP_TIMEOUT (in seconds).  


##### (BUG-94801) New Default Images Including OS with Spectre and Meltdown Patches 

All default images used by Cloudbreak were rebuilt. Each image now includes an updated version of the operating system with the following CPU security fixes: 

* Cloudbreak image (Centos 7): Moved to Centos version 7.4.1708, which includes fixes for CVE-2017-5753 (also known as "Spectre Variant 1") and "Meltdown" (also known as "Variant 3").  
* Default HDP image for Azure, Google Cloud, and Openstack (Centos 7): Moved to Centos version 7.4.1708, which includes fixes for CVE-2017-5753 (also known as "Spectre Variant 1") and "Meltdown" (also known as "Variant 3").  
* Default HDP image for AWS (Amazon Linux): Moved to Amazon Linux 2017.09 version, kernel version 4.9.81-35, which includes fixes for CVE-2017-5753 (also known as "Spectre Variant 1"), CVE-2017-5715 (also known as "Spectre Variant 2"), and "Meltdown" (also known as "Variant 3").  





____________________________

#### Image Catalog Updates 
____________________________

##### March 16, 2018 

[Comment]: <> (Update the date once the update is available.)

Update 1:

Default Ambari version 2.6.1.3  
Default HDP version 2.6.4.5-2   

##### February 23, 2018

Default Ambari version 2.6.1.3   
Default HDP version  2.6.4.0-91    





____________________________

#### Known Issues
____________________________

**Known Issues: Cloudbreak**
____________________________







##### (BUG-BUG-99168) All Google Cloud Clusters Fail 

All clusters launched on Google Cloud Platform fail. This happens due to a recent change in the Google Cloud Platform's response code. In order to fix the issue, a change Cloudbreak's code was required. The change was introduced in Cloudbreak 2.4.1.

*Workaround*: 

There is no workaround for this in Cloudbreak 2.4.0. In order to create clusters on Google Cloud Platform, you must [launch](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cloudbreak-2.4.1/content/gcp-launch/index.html) or [upgrade](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cloudbreak-2.4.1/content/cb-upgrade/index.html) to Cloudbreak 2.4.1.   

  
____________________________






##### (BUG-96788) Azure Availability Set Option Is Not Available for Instance Count of 1

When creating a cluster, the Azure availability set feature is not available for host groups with the instance count of 1.

*Workaround*: 

This issue will be fixed in a future release. 

If you would like to use the Azure availability sets feature now, you must add at least 2 instances to the host group for which you want to use them. The option Azure availability sets is available on the advanced **Hardware and Storage** page of the create cluster wizard.   
____________________________







##### (BUG-92605) Cluster Creation Fails with ResourceInError

Cluster creation fails with the following error: 

*Infrastructure creation failed. Reason: Failed to create the stack for CloudContext{id=3689, name='test-exisitngnetwork', platform='StringType{value='OPENSTACK'}', owner='e0307f96-bd7d-4641-8c8f-b95f2667d9c6'} due to: Resource CREATE failed: ResourceInError: resources.ambari_volume_master_0_0: Went to status error due to "Unknown"*

*Workaround*: 

This may mean that the volumes that you requested exceed volumes available on your cloud provider account. When creating a cluster, on the advanced **Hardware and Storage** page of the create cluster wizard, try reducing the amount of requested storage. If you need more storage, try using a different region or ask your cloud provider admin to increase the resource quota for volumes.  

[Comment]: <> (This jira item was closed so it will not be fixed. Maybe add this to troubleshooting?)
____________________________








##### (BUG-91013) Incorrect Node Status After Cluster Restart 

You may sporadically experience an issue where after you stop and restart a cluster, the node status displayed in the "Hardware" section is incorrect.   

[comment]: <> (Not sure what the workaround is for BUG-91013?)
____________________________





##### (BUG-?????) Time Based Autoscaling Does Not Work 

Time based cluster autoscaling does not work. This issue will be fixed in the upcoming release. 
____________________________







##### (BUG-93241) Error When Scaling Multiple Host Groups 

Scaling of multiple host groups fails with the following error: 

*Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1; nested exception is org.hibernate.StaleStateException: Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1*

*Workaround:*

Scaling multiple host groups at once is not supported. If you would like to scale multiple host groups: scale the first host group and wait until scaling has completed, then scale the second host group, and so on.  
____________________________







##### (BUG-96764) "Failed to remove instance" Error When Using the Delete Icon

When trying to delete instances by using the delete icon on the cluster details in the **Hardware** tab, you will get the following error: "Failed to remove instance".

*Workaround:*

This issue will be fixed in a future release. If you would like to change the number of instances in a given host group, you can use the **Resize** option available from the **Actions** menu.  
____________________________









##### (BUG-97044) Show CLI Command Copy JSON Button Does Not Work
  
When using the **Show CLI Command** > **Copy the JSON** or **Copy the Command** button with  Firefox, the content does not does not get copied if adblock plugin or other advertise blocker plugins are present.

 *Workaround:*  
 
 Use a browser without an adblock plugin. 
____________________________







##### (BUG-95607) Special Characters in Blueprint Name Cause an Error in CLI  

When registering a blueprint via `blueprint create` CLI command, if the name of the blueprint includes one or more of the following special characters `@#$%|:&*;` you will get an error similar to:  

<pre>cb blueprint create from-url --name test@# --url https://myurl.com/myblueprint.bp  
[1] 7547
-bash: application.yml: command not found
-bash: --url: command not found
 ~  integration-test  1  time="2018-02-01T12:56:44+01:00" level="error" msg="the following parameters are missing: url\n"</pre> 
 
 *Workaround:*  
You must use quotes when specifying a blueprint name which includes special characters:  
<pre>cb blueprint create from-url --name "test@#" --url https://myurl.com/myblueprint.bp
____________________________






##### (BUG-93257) Clusters Are Missing From History   

After changing the dates on the History page multiple times, the results displayed may sometimes be incorrect. 

*Workaround:*

Refresh the page if you think that the history displayed may be incorrect.  
____________________________







##### (AMBARI-14149) Ambari Cluster Cannot Be Started After Stop

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

**Known Issues: Ambari 2.6.1.3 and HDP 2.6.4.0**

> The known issues described here were discovered when testing Cloubdreak with Ambari 2.6.1.3 and HDP 2.6.4.0, which are used by default in Cloudbreak 2.4.0.

> For general Ambari 2.6.1.3 and HDP 2.6.4.0 known issues, refer to:  
> [Ambari 2.6.1.3 Release Notes](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.3/bk_ambari-release-notes/content/ch_relnotes-ambari-2.6.1.3.html)  
> [HDP 2.6.4.0 Release Notes](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.4/bk_release-notes/content/ch_relnotes.html)  
____________________________






##### (BUG-96784) Hive LLAP Start Takes More Than an Hour

Hive LLAP start times out in Ambari, but eventually it starts (after 15 minutes on AWS and after one hour or so on other providers).  
____________________________







##### (BUG-96707) Druid Overload Does Not Start

Druid overload start fails with the following error when using Ambari 2.6.1.3 and HDP 2.6.4.0: 

*ERROR [main] io.druid.cli.CliOverlord - Error when starting up.  Failing. com.google.inject.ProvisionException: Unable to provision* 
  
____________________________








##### (BUG-97080) Ambari Fils In Some Cases When an mpack is Installed 

If we set the following properties then cluster install may fail (in 20-30% of the cases), because of the Ambari agent cache being updated concurrently:

<pre>/etc/ambari-server/conf/ambari.properties  
agent.auto.cache.update=true*  
*/etc/ambari-agent/conf/ambari-agent.ini  
parallel_execution=1</pre> 
____________________________



