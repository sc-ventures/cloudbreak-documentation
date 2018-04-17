## Using management packs

TBD

https://cwiki.apache.org/confluence/display/AMBARI/Management+Packs 

### Add management pack

In order to have a management stack installed for a specific cluster, you must register it first by using the steps below.

**Steps**

1. In CLoudbreak web UI, select **External Sources > Management Packs** from the navigation menu. 

3. Click on **Register Management Pack**. 

4. Provide the following:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your managament pack. |
| Description | (Optional) Enter a description.|
| Management pack URL | Provide the URL to the location where the management pack tarball is available. |
| Remove all existing Ambari stack definitions prior to installing this Management Pack (“mpack --purge”).
| Checking this option allows you to purge any existing stack definition and should be included only when installing a stack management pack. Do not select this when installing an add-on service management pack. |
    
4. When creating a cluster, you can select previously added management packs on the advanced **Cluster Extensions** page of the cluster wizard. 



