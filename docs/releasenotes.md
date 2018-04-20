## Release notes

### 2.6.0 TP

Cloudbreak 2.6.0 is a technical preview release. 

____________________________

#### New features
____________________________

##### **Using MySQL and Oracle External Databases** 

Cloudbreak 2.6.0 TP introduces support for creating external MySQL and Oracle databases, in addition to previously supported Postgres. For more information, refer to [Using an external database](external-db.md).   
____________________________
 
##### **Creating HDF Messaging Clusters** 

Cloudbreak 2.6.0 TP introduces the ability to create HDF Messaging clusters, including Apache Kafka. To help you get started, Cloudbreak provides a new built-in **HDF Messaging: Apache Kafka** blueprint.  
For the list of available blueprints, refer to [Default Cluster Configurations](index.html#default-cluster-configurations).  
____________________________

##### **Creating Flow Management Clusters with NiFi Registry**

Cloudbreak 2.6.0 TP adds support for NiFi Registry and NiFi Registry was added to the default **Flow Management: Apache NiFi** blueprint. 

When creating a NiFI cluster, make sure to do the following:

* If using the default blueprint, place the Ambari Server on the "Services" host group.     
* When creating a cluster, open 9091 TCP port on the NiFi host group. Without it, you will be unable to access the NiFi web UI.   
* When creating a cluster, open port 61443 on the Services host group. This port is used by NiFi Registry.      
* When creating the NiFi Registry controller service in NiFi, the internal hostname has to be used, `e.g. https://ip-1-2-3-4.us-west-2.compute.internal:61443`   
* Enabling kerberos is mandatory. You can either use your own kerberos or select for Cloudbreak to create a test KDC.  
* Although Cloudbreak includes cluster scaling (including autoscaling), scaling is not supported by NiFi. Downscaling NiFi clusters is not supported - as it can result in data loss when a node is removed that has not yet processed all the data on that node. There is also a known issue related to scaling listed in the [Known Issues](#known-issues) below.  

For the list of available blueprints, refer to [Default Cluster Configurations](index.html#default-cluster-configurations).  
To get started creating NiFi clusters, refer to the following [HCC post](https://community.hortonworks.com/articles/182221/create-a-nifi-cluster-on-aws-azure-google-or-opens.html).  
____________________________

##### **Using Management Packs** 

Cloudbreak 2.6.0 TP introduces support for using management packs, allowing you to register them in Cloudbreak web UI or CLI and then select to install them as part of cluster creation.  
For more information, refer to [Using management packs](mpacks.md).  
____________________________

##### **Technical Preview of HDP 3.0** 

Cloudbreak 2.6.0 TP includes one default HDP 3.0 blueprint, allowing you to try HDP 3.0.  
For the list of available blueprints, refer to [Default Cluster Configurations](index.html#default-cluster-configurations).


____________________________

#### Behavioral changes
____________________________

##### **Image Catalog Option Moved to External Sources**

The options related to registering a custom image catalog and selecting a default image catalog were removed from the **Settings** navigation menu option and are now available under **External Sources > Image Catalogs**.
____________________________
  
##### **Recipes Menu Option Moved to Cluster Extensions**

The **Recipes** navigation menu option was moved under **Cluster Extensions**, so to find recipe-related settings, select **Cluster Extensions > Recipes** from the navigation menu. 






____________________________
 
#### Image catalog updates
____________________________
 
##### **April 3, 2018** 

Update 1:

Default Ambari version 2.6.1.3  
Default HDP version 2.6.4.5-2  
Default HDF version 3.1.1.0-35  
____________________________ 

##### **February 23, 2018**

Default Ambari version 2.6.1.3   
Default HDP version  2.6.4.0-91  
Default HDF version 3.1.1.0-35   






____________________________

#### Fixed issues
____________________________



[Comment]: <> (BUG-99505 is still in progress. If not done, should be removed from this list.)
[Comment]: <> (BUG-98792 is also tagged for 2.4.1 but is still open. If done, it should be added to this list.)
[Comment]: <> (BUG-97207 is internal? YARN?)
[Comment]: <> (Not included in 2.5.0 TP, only 2.4.1: BUG-99635 After deleting a default credential and creating a new credential, credential is missing from the create cluster wizard.)

| Issue | Issue description | Category | Fix version | 
|---|---|---|---|
| BUG-99168 | All clusters created on Google Cloud Platform fail. | Stability | 2.5.0 TP |
| BUG-99400 | Time-based cluster autoscaling does not work. | Stability | 2.5.0 TP |
| BUG-99505 | Sync is not working for an AWS instance that was terminated a long time ago. | Stability | 2.5.0 TP |
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

**Known issues: Ambari 2.6.1.3 and HDP 2.6.4.0**

> The known issues described here were discovered when testing Cloudbreak with Ambari 2.6.1.3 and HDP 2.6.4.0, which are used by default in Cloudbreak.

> For general Ambari 2.6.1.5 and HDP 2.6.4.0 known issues, refer to:  
> [Ambari 2.6.1.5 Release Notes](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.5/bk_ambari-release-notes/content/ch_relnotes-ambari-2.6.1.5.html)  
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

**Known issues: HDF 3.1.1**

The known issues described here were discovered when testing Cloudbreak with Ambari 2.6.1.3 and HDF 3.1.1, which are used by default in Cloudbreak.

> For general HDF 3.1.1 known issues, refer to [HDF 3.1.1 Release Notes](https://docs.hortonworks.com/HDPDocuments/HDF3/HDF-3.1.1/bk_release-notes/content/ch_hdf_relnotes.html)

____________________________



##### (BUG-98865) Scaling HDF Clusters Does Not Update Configurations on New Nodes

Blueprint configuration parameters are not applied when scaling an HDF cluster. 
One example that affects all users is that after HDF cluster upscale/downscale the `nifi.web.proxy.host` blueprint parameter does not get updated to include the new nodes, and as a result the NiFi UI is not reachable from these nodes. 

*Workaround:*  
 
Configuration parameters set in the blueprint are not applied when scaling an HDF cluster. One example that affects all NiFi users is that after HDF cluster upscale the `nifi.web.proxy.host` parameter does not get updated to include the new hosts, and as a result the NiFi UI is not reachable from these hosts. 
 
`HOST1-IP:PORT,HOST2-IP:PORT,HOST3-IP:PORT`
   



