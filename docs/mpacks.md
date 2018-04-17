## Using management packs

Management packs allow you to deploy a range of services to your Ambari-managed cluster. You can use a management pack to deploy a specific component or service, like HDP Search, or to deploy an entire platform, like HDF.

Cloudbreak supports using management packs, allowing you to register them in Cloudbreak web UI and CLI and then select to install them as part of cluster creation. 

For general information on management packs, refer to [Ambari cwiki: Management+Packs](https://cwiki.apache.org/confluence/display/AMBARI/Management+Packs).  

**Related Links**  
[Ambari cwiki: Management+Packs](https://cwiki.apache.org/confluence/display/AMBARI/Management+Packs)  


### Add management pack

In order to have a management stack installed for a specific cluster, you must register it with Cloudbreak by using the steps below.

**Steps**

1. Obtain the URL for the management pack tarball file that you want to register in Cloudbreak. The tarball  must be available in a location accessible to clusters created by Cloudbreak.  

1. In Cloudbreak web UI, select **External Sources > Management Packs** from the navigation menu. 

3. Click on **Register Management Pack**. 

4. Provide the following:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your managament pack. |
| Description | (Optional) Enter a description.|
| Management pack URL | Provide the URL to the location where the management pack tarball file is available. |
| Remove all existing Ambari stack definitions prior to installing this Management Pack (“mpack --purge”).
| Checking this option allows you to purge any existing stack definition and should be included only when installing a stack management pack. Do not select this when installing an add-on service management pack. |
    
4. When creating a cluster, on the advanced **Cluster Extensions** page of the create cluster wizard, you can select one or more previously registered management packs. After selecting, click **Attach** to use the management pack for the cluster. 



