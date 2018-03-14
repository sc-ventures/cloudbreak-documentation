## Release Notes

### 2.5.0 TP

Cloudbreak 2.5.0 is a technical preview release. 

____________________________

#### New Features
____________________________

##### Creating HDF Clusters 

You can use Cloudbreak to create HDF clusters on AWS, Azure, Google Cloud and OpenStack. In the Cloudbreak web UI, you can do this by selecting "HDF 3.1" under Platform Version and then selecting an HDF blueprint.

Cloudbreak includes one default HDF blueprint "Flow Management: Apache Nifi" and supports uploading your own custom blueprints. This feature supports HDF 3.1.1 blueprints. HDF clusters are currently launched from base images; prewarmed images are not available.  

For updated create cluster instructions, refer to [Creating a Cluster](aws-launch.md) instructions for your chosen cloud provider.
For updated blueprint information, refer to [Default Blueprints](blueprints.md#default-blueprints). 


##### Using External RDS for Cluster Services 

You can register an existing external RDS in the Cloudbreak UI or CLI so that the external RDS can be used for those cluster components which support using an external RDS. After the RDS has been registered in the Cloudbreak UI, it is available during the cluster create and can be reused with one or more clusters.  

Only Postgre is supported at this time. Refer to component-specific documentation for information on which version of Postgres (if any) is supported.

Cloudbreak does not configure the external RDS for you, the RDS must exist prior to registering it with Cloudbreak. 


##### Configuring LDAP/AD for Clusters 

You can configure Cloudbreak to create an LDAP/AD that can later be associated with Cloudbreak-managed clusters.


##### Editing Existing Cloudbreak Credentials 

Cloudbreak allows you to edit existing credentials by using the edit option available in Cloudbreak UI or CLI. 


##### Launching Cloudbreak in Environments with Limited or No Internet Access 

You can launch Cloudbreak in environments with limited internet access or no internet access. 


##### Configuring Existing LDAP for Cloudbreak

You can configure Cloudbreak to use your existing LDAP/AD so that you can authenticate Cloudbreak users against an existing LDAP/AD server. 


##### Auto-import of HDP/HDF Images on OpenStack

When using Cloudbreak on OpenStack, you no longer need to import HDP and HDF images manually, because during your first attempt to create a cluster, Cloudbreak automatically imports HDP and HDF images to your OpenStack. Only Cloudbreak image must be imported manually. 

____________________________

#### Behavioral Changes
____________________________


____________________________
 
#### Image Catalog Updates
____________________________
 
 
##### March 16, 2018 

[Comment]: <> (Update the date once the update is available.)

Update 1:

Default Ambari version 2.6.1.3  
Default HDP version 2.6.4.5-2  
Default HDF version 3.1.1.0-35   

##### February 23, 2018

Default Ambari version 2.6.1.3   
Default HDP version  2.6.4.0-91    


____________________________

#### Fixed Issues 
____________________________


____________________________

#### Known Issues
____________________________

**Known Issues: Cloudbreak**
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

<pre>cb blueprint create from-url --name test@# --url https://myurl.com/myblueprint.bp  
[1] 7547
-bash: application.yml: command not found
-bash: --url: command not found
 ~  integration-test  1  time="2018-02-01T12:56:44+01:00" level="error" msg="the following parameters are missing: url\n"</pre> 
 
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



