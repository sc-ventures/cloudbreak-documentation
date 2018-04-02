## Release Notes

### 2.5.0 TP

Cloudbreak 2.5.0 is a technical preview release. 

____________________________

#### New Features
____________________________

##### **Creating HDF Clusters** 

You can use Cloudbreak to create HDF clusters from base images on AWS, Azure, Google Cloud and OpenStack. In the Cloudbreak web UI, you can do this by selecting "HDF 3.1" under *Platform Version* and then selecting an HDF blueprint.

Cloudbreak includes one default HDF blueprint "Flow Management: Apache NiFi" and supports uploading your own custom HDF 3.1.1 NiFi blueprints.  

Note the following when creating NiFi clusters:

- When creating a cluster, open 9091 TCP port on the NiFi host group. Without it, you will be unable to access the UI.  
- Enabling kerberos is mandatory. You can either use your own kerberos or select for Cloudbreak to create a test KDC.    
- Although Cloudbreak includes cluster scaling (including autoscaling), scaling is not fully supported by NiFi. Downscaling NiFi clusters is not supported - as it can result in data loss when a node is removed that has not yet processed all the data on that node. There is also a known issue related to scaling listed in the [Known Issues](#known-issues) below.  

For updated create cluster instructions, refer to [Creating a Cluster](aws-launch.md) instructions for your chosen cloud provider.
For updated blueprint information, refer to [Default Blueprints](blueprints.md#default-blueprints). 



##### **Using External Databases for Cluster Services** 

You can register an existing external RDBMS in the Cloudbreak UI or CLI so that it can be used for those cluster components which have support for it. After the RDBMS has been registered with Cloudbreak, it will be available during the cluster create and can be reused with multiple clusters.  

Only Postgres is supported at this time. Refer to component-specific documentation for information on which version of Postgres (if any) is supported.  

For more information, refer to [Register an External Database](external-db.md).   


##### **Using External Authentication Sources (LDAP/AD) for Clusters** 

You can configure an existing LDAP/AD authentication source in the Cloudbreak UI or CLI so that it can later be associated with one or more Cloudbreak-managed clusters. After the authentication source has been registered with Cloudbreak, it will be available during the cluster create and can be reused with multiple clusters.

For more information, refer to [Register an Authentication Source](external-ldap.md).     


##### **Modifying Existing Cloudbreak Credentials** 

Cloudbreak allows you to modify existing credentials by using the edit option available in Cloudbreak UI or CLI. For more information, refer to [Modify an Existing Credential](cb-credentials.md#modify-an-existing-credential). 


##### **Configuring Cloudbreak to Use Existing LDAP/AD**

You can configure Cloudbreak to use your existing LDAP/AD so that you can authenticate Cloudbreak users against an existing LDAP/AD server. For more information, refer to [Configuring Cloudbreak for LDAP/AD Authentication](cb-ldap.md). 


##### **Launching Cloudbreak in Environments with Limited or No Internet Access** 

You can launch Cloudbreak in environments with limited internet access or no internet access. For more information, refer to [Configure Outbound Internet Access and Proxy](cb-proxy.md). 

____________________________

#### Behavioral Changes
____________________________


##### **Auto-import of HDP/HDF Images on OpenStack**

When using Cloudbreak on OpenStack, you no longer need to import HDP and HDF images manually, because during your first attempt to create a cluster, Cloudbreak automatically imports HDP and HDF images to your OpenStack. Only Cloudbreak image must be imported manually. 

____________________________
 
#### Image Catalog Updates
____________________________
 
 
##### **April 3, 2018** 

[Comment]: <> (Update the date once the update is available.)

Update 1:

Default Ambari version 2.6.1.3  
Default HDP version 2.6.4.5-2  
Default HDF version 3.1.1.0-35   

##### **February 23, 2018**

Default Ambari version 2.6.1.3   
Default HDP version  2.6.4.0-91  
Default HDF version 3.1.1.0-35   






____________________________

#### Fixed Issues
____________________________

[Commmet]: <> (BUG-99505 is still in progress. If not done, should be removed from this list.)
[Commmet]: <> (BUG-98792 is also tagged for 2.4.1 but is still open. If done, it should be added to this list.)
[Comment]: <> (BUG-97207 is internal? YARN?)

| Issue | Issue Description | Category | 
|---|---|
| BUG-99168 | All clusters created on Google Cloud Platform fail. | Stability | 
| BUG-99400 | Time-based cluster autoscaling does not work. | Stability |
| BUG-99505 | Sync is not working for an AWS instance that was terminated a long time ago. | Stability |
| BUG-98277 | Network interface handling in CloudBreak should be improved. | Stability |
| BUG-97395 | Networks are duplicated on networks tab of the cluster create wizard. | Stability |
| BUG-97259 | "Update failed" status after downscale failed, even though cluster was not modified and its status should be "Running". | Stability |
| BUG-97207 | Changing lifecycle management on YARN causes NPE. | Stability |
| BUG-99189 | ImageCatalog PUT endpoint is not secured. | Security |
| BUG-97895 | LDAP password should be removed from Cloudbreak logs. | Security |
| BUG-97300 | Cloudbreak should show proper error messages when the given credential is not valid anymore. | Usability |
| BUG-97296| GCP credential creation should validate whether resources are available with the credential. | Usability |
| BUG-99635 | Default credential selection problem. | Usability |
| BUG-97660 | Ignore repository warnings checkbox are missing after changing base image Ambari or HDP to a custom one. | Usability |
| BUG-97307 | Ignore repository warnings checkbox is not selectable after change the HDP VDF URL. | Usability |
| BUG-96764 | "Failed to remove instance" error when using the delete icon. | Usability | 
| BUG-97390 | Cloudbreak should support longer resource ID-s on AWS. | Usability |
| BUG-99512 | Azure ES_v3 instances should support premium storage. | Usability | 
| BUG-97206 | Backend should return only images for enabled platforms. | Usability |







____________________________

#### Known Issues
____________________________

**Known Issues: Cloudbreak**
____________________________




##### (BUG-96788) **Azure Availability Set Option Is Not Available for Instance Count of 1**

When creating a cluster, the Azure availability set feature is not available for host groups with the instance count of 1.

*Workaround*: 

This issue will be fixed in a future release. 

If you would like to use the Azure availability sets feature now, you must add at least 2 instances to the host group for which you want to use them. The option Azure availability sets is available on the advanced **Hardware and Storage** page of the create cluster wizard.   
____________________________







##### (BUG-92605) **Cluster Creation Fails with ResourceInError**

Cluster creation fails with the following error: 

*Infrastructure creation failed. Reason: Failed to create the stack for CloudContext{id=3689, name='test-exisitngnetwork', platform='StringType{value='OPENSTACK'}', owner='e0307f96-bd7d-4641-8c8f-b95f2667d9c6'} due to: Resource CREATE failed: ResourceInError: resources.ambari_volume_master_0_0: Went to status error due to "Unknown"*

*Workaround*: 

This may mean that the volumes that you requested exceed volumes available on your cloud provider account. When creating a cluster, on the advanced **Hardware and Storage** page of the create cluster wizard, try reducing the amount of requested storage. If you need more storage, try using a different region or ask your cloud provider admin to increase the resource quota for volumes.  

[Comment]: <> (This jira item was closed so it will not be fixed. Maybe add this to troubleshooting?)
____________________________










##### (BUG-93241) **Error When Scaling Multiple Host Groups** 

Scaling of multiple host groups fails with the following error: 

*Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1; nested exception is org.hibernate.StaleStateException: Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1*

*Workaround:*

Scaling multiple host groups at once is not supported. If you would like to scale multiple host groups: scale the first host group and wait until scaling has completed, then scale the second host group, and so on.  
____________________________













##### (BUG-97044) **Show CLI Command Copy JSON Button Does Not Work**
  
When using the **Show CLI Command** > **Copy the JSON** or **Copy the Command** button with  Firefox, the content does not does not get copied if adblock plugin or other advertise blocker plugins are present.

 *Workaround:*  
 
 Use a browser without an adblock plugin. 
 
____________________________












##### (BUG-93257) **Clusters Are Missing From History**   

After changing the dates on the History page multiple times, the results displayed may sometimes be incorrect. 

*Workaround:*

Refresh the page if you think that the history displayed may be incorrect.  








____________________________

**Known Issues: Ambari 2.6.1.3 and HDP 2.6.4.0**

> The known issues described here were discovered when testing Cloubdreak with Ambari 2.6.1.3 and HDP 2.6.4.0, which are used by default in Cloudbreak.

> For general Ambari 2.6.1.3 and HDP 2.6.4.0 known issues, refer to:  
> [Ambari 2.6.1.3 Release Notes](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.3/bk_ambari-release-notes/content/ch_relnotes-ambari-2.6.1.3.html)  
> [HDP 2.6.4.0 Release Notes](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.4/bk_release-notes/content/ch_relnotes.html)  
____________________________









##### (BUG-96707) **Druid Overload Does Not Start**

Druid overload start fails with the following error when using Ambari 2.6.1.3 and HDP 2.6.4.0: 

*ERROR [main] io.druid.cli.CliOverlord - Error when starting up.  Failing. com.google.inject.ProvisionException: Unable to provision* 
  
____________________________








##### (BUG-97080) **Ambari Fils In Some Cases When an mpack is Installed** 

If we set the following properties then cluster install may fail (in 20-30% of the cases), because of the Ambari agent cache being updated concurrently:

<pre>/etc/ambari-server/conf/ambari.properties  
agent.auto.cache.update=true*  
*/etc/ambari-agent/conf/ambari-agent.ini  
parallel_execution=1</pre> 

[Comment]: <> (Fixed in Ambari 2.7) 

____________________________







##### (AMBARI-14149) **Ambari Cluster Cannot Be Started After Stop**

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

**Known Issues: HDF 3.1.1**

The known issues described here were discovered when testing Cloubdreak with Ambari 2.6.1.3 and HDF 3.1.1, which are used by default in Cloudbreak.

> For general HDF 3.1.1 known issues, refer to [HDF 3.1.1 Release Notes](https://docs.hortonworks.com/HDPDocuments/HDF3/HDF-3.1.1/bk_release-notes/content/ch_hdf_relnotes.html)

____________________________



##### (BUG-98865) Scaling HDF Clusters Does Not Update Configurations on New Nodes

Blueprint configuration parameters are not applied when scaling an HDF cluster. 
One example that affects all users is that after HDF cluster upscale/downscale the `nifi.web.proxy.host` blueprint parameter does not get updated to include the new nodes, and as a result the Nifi UI is not reachable from these nodes. 

*Workaround:*  
 
Configuration parameters set in the blueprint are not applied when scaling an HDF cluster. One example that affects all NiFi users is that after HDF cluster upscale the `nifi.web.proxy.host` parameter does not get updated to include the new hosts, and as a result the Nifi UI is not reachable from these hosts. 
 
`HOST1-IP:PORT,HOST2-IP:PORT,HOST3-IP:PORT`
   



