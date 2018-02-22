## Release Notes

### 2.4.0

Cloudbreak 2.4.0 is a general availability release, which is suitable for production deployments. 





____________________________

#### New Features
____________________________

##### New UI/UX

Cloudbreak 2.4.0 introduces a new user interface:

<a href="../images/cb-ui3.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui3.png" width="650" title="Cloudbreak web UI"></a>  

All major options are now easily accessible from the collapsable navigation menu on the left side. All UI options and wizards have been redesigned in order to make cluster creation and management more intuitive. 
____________________________


##### New CLI

Cloudbreak 2.4.0 introduces a new CLI tool. All commands start with a singular object followed by an action, for example, `blueprint create` instead of `create blueprint`, and `blueprint list` instead of `list blueprints`. Refer to [Install CLI](cli-install.md) and [CLI Reference](cli-reference.md).
____________________________


##### Support for Kerberos

Creating Kerberos-enabled clusters is supported:  

- Use an existing existing MIT KDC or Active Directory for production deployments.  
- Set up a test KDC for evaluation purposes.  

Refer to [Enabling Kerberos Security](security-kerberos.md).
____________________________


##### Support for Configuring an External RDBMS for Cloudbreak

Using an external RDBMS for Cloubdreak is supported and recommended for production environments. For configuration instructions, refer to [Configuring External Cloudbreak Database](cb-db.md).
____________________________


##### Support for Migrating Cloudbreak Instance

Migrating Cloudbreak from one machine to another is supported. For migration instructions, refer to [Moving a Cloudbreak Instance](cb-migrate.md).
____________________________


##### Prewarmed Images 

To accelerate cluster creation, CLoudbreak 2.4.0 introduces prewarmed images, which include the operating system, as well as the default version of Ambari and HDP. By default, Cloudbreak 2.4 launches clusters from these prewarmed images, instead of using base images (which were used by default in earlier versions of Cloudbreak). Default base images are still available in case you would like to use different Ambari and HDP versions than those provided with prewarmed images. For more information, refer to [Prewardmed and Base Images](aws-create.md#prewarmed-and-base-images). 
____________________________


##### Providing Your Own JDK

Providing your own JDK on a custom base image is supported. For instructions, refer to "Advanced topics" in the [https://github.com/hortonworks/cloudbreak-images](https://github.com/hortonworks/cloudbreak-images) repository.
____________________________


##### Generating CLI Templates 

After specifying the parameters for your cluster in the Cloudbreak web UI, you can copy the content of the CLI JSON file that can be used to create a cluster via Cloudbreeak CLI. For more information, refer to [Obtain Cluster JSON Template from the UI](cli-get-started.md#obtain-cluster-json-template-from-the-ui).    

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
- To enable LZO compression in your HDP cluster, you must check the "Enable Ambari Server to download and install GPL Licensed LZO packages?" during cluster creation. The option is available in the **Security** > **Prewarmed and Base Images**.  

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

#### Fixed Issues 
____________________________





##### (BUG-94801) New Default Images Including OS Fixes for CPU Security Issues 

All default images used by Cloudbreak were rebuilt. Each image now includes an updated version of the operating system with the following CPU security fixes: 

* Cloudbreak image (Centos 7): Moved to Centos version 7.4.1708, which includes fixes for Spectre Variant 1 and 3.  
* Default HDP image for AWS (Amazon Linux): Moved to Amazon Linux 2017.09 version, which includes fixes for Spectre Variant 1, 2, and 3.  
* Default HDP image for Azure, Google Cloud, and Openstack (Centos 7): Moved to Centos version 7.4.1708, which includes fixes for Spectre Variant 1 and 3.  





____________________________

#### Known Issues

The known issues were discovered when testing Ambari 2.6.1.3 and HDP 2.6.4.0 which are used by default in Cloudbreak 2.4.0.
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

*cb blueprint create from-url --name test@# --url https://myurl.com/myblueprint.bp  
[1] 7547
-bash: application.yml: command not found
-bash: --url: command not found
 ~  integration-test  1  time="2018-02-01T12:56:44+01:00" level="error" msg="the following parameters are missing: url\n"* 
 
 *Workaround:*  
 You have two options:
 
* Do not include any of the following special characters `@#$%|:&*;` in the blueprint name.  
* If you want to use special characters in the name, perform the task via the UI.  
____________________________






##### (BUG-93257) Clusters Are Missing From History   

After changing the dates on the History page multiple times, the results displayed may sometimes be incorrect. 

*Workaround:*

Refresh the page if you think that the history displayed may be incorrect.  
____________________________







##### (AMBARI-14149) Ambari Cluster Cannot Be Started After Stop

When using Ambari version 2.5.0.3, after stopping and starting a cluster, Event History shows the following error:

*Ambari cluster could not be started. Reason: Failed to start Hadoop services.
2/7/2018, 12:47:05 PM
Starting Ambari services.
2/7/2018, 12:47:04 PM
Manual recovery is needed for the following failed nodes:   
[host-10-0-0-4.openstacklocal, host-10-0-0-3.openstacklocal, host-10-0-0-5.openstacklocal*

Ambari dashboard shows that nodes are not sending heartbeats. 

 *Workaround:*  
 
 This issue is fixed in Ambari version 2.5.1.0 and newer.  

[Comment]: <> (See BUG-96086, EAR-6780, AMBARI-14149)
____________________________







##### (BUG-96784) Hive LLAP Start Takes More Than an Hour

Hive LLAP start times out in Ambari, but eventually it starts (after 15 minutes on AWS and after one hour or so on other providers).  
____________________________







##### (BUG-96707) Druid Overload Does Not Start

Druid overload start fails with *ERROR [main] io.druid.cli.CliOverlord - Error when starting up.  Failing. com.google.inject.ProvisionException: Unable to provision* when using Ambari 2.6.1.3 and HDP 2.6.4.0.  
____________________________







##### (BUG-94210) Ambari Fails to Replace HOSTGROUP var for Schema Registry and Streamline

Ambari fails to replace HOSTGROUP var for Schema Registry and Streamline in the blueprint: 

*"registry.url": "http://%HOSTGROUP::master_2%:7788/api/v1,http://%HOSTGROUP::master_3%:7788/api/v1",  
"streamline.dashboard.url": "http://%HOSTGROUP::master_2%:9090",  
"registry.storage.connector.connectURI": "jdbc:mysql://%HOSTGROUP::master_2%:3306/registry"*
____________________________






##### (BUG-97055) Name Node Goes into Safe Mode Frequently 

Name Node goes into safe mode very frequently. After turning safe mode off manually, the Name Node continues running normally, but after a while it goes into safe mode again.
____________________________








##### (BUG-97080) Ambari Fils In Some Cases When an mpack is Installed 

If we set the following properties then cluster install may fail (in 20-30% of the cases), because of the Ambari agent cache being updated concurrently:

*/etc/ambari-server/conf/ambari.properties
agent.auto.cache.update=true*

*/etc/ambari-agent/conf/ambari-agent.ini
parallel_execution=1*
____________________________






##### (BUG-97066) DLM 1.0 mpack Install Fails

When installing DLM 1.0 mpack:
* Ranger install fails. 
* When BEACON_SERVER is present in the blueprint, all worker nodes fail. 

As a result the entire HDP installation fails.  
____________________________







##### (BUG-91984) Ranger Fails to Install from Blueprint

Ranger install from Ambari Blueprint fails with with DB access error:

<pre>17 15:40:00,148  [I] Running DBA setup script. QuiteMode:True
2017-11-17 15:40:00,149  [I] Using Java:/usr/lib/jvm/java/bin/java
2017-11-17 15:40:00,149  [I] DB FLAVOR:MYSQL
2017-11-17 15:40:00,149  [I] DB Host:
2017-11-17 15:40:00,149  [I] ---------- Verifying DB root password ---------- 
2017-11-17 15:40:00,150  [I] DBA root user password validated
2017-11-17 15:40:00,150  [I] ---------- Verifying Ranger Admin db user password ---------- 
2017-11-17 15:40:00,150  [I] admin user password validated
2017-11-17 15:40:00,150  [I] ---------- Creating Ranger Admin db user ---------- 
2017-11-17 15:40:00,150  [JISQL] /usr/lib/jvm/java/bin/java  -cp /usr/hdp/current/ranger-admin/ews/lib/mysql-connector-java-5.1.39-bin.jar:/usr/hdp/current/ranger-admin/jisql/lib/* org.apache.util.sql.Jisql -driver mysqlconj -cstring jdbc:mysql:///mysql -u root -p '********' -noheader -trim -c \; -query "SELECT version();"
SQLException : SQL state: 28000 java.sql.SQLException: Access denied for user 'root'@'localhost' (using password: YES) ErrorCode: 1045
2017-11-17 15:40:00,714  [E] Can't establish db connection.. Exiting..
Traceback (most recent call last):</pre>
____________________________