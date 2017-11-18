## Access Your Cluster

The following section describes how to access the various services in the cluster.


### Find Cluster Information in the UI

Once your cluster is up and running, click on the tile representing your cluster in the Cloudbreak UI to access information related the cluster and access cluster actions. 

<a href="../images/cb-ui-clinfo.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui-clinfo.png" width="650" title="Cluster Information"></a> 

The information presented includes:

* [Cluster Summary](#cluster-summary)
* [Cluster Information](#cluster-information) 
* [Hardware](#hardware)
* [Tags](#tags) 
* [Event History](#event-history) 

<div class="note">
  <p class="first admonition-title">Tips</p>
  <p class="last"><ul>
  <li> Access cluster actions such as resize and sync by clicking on <b>ACTIONS</b>.</li>
  <li> Access Ambari web UI by clicking on the link in the <b>CLUSTER INFORMATION</b> section.</li>
<li> View public IP addresses for all cluster instances in the <b>HARDWARE</b> section. Click on the links to view the instances in the cloud console.</li>
<li> The SSH user that you must use when accessing cluster VMs is "cloudbreak".</li> 
</ul>
</p>
</div>

#### Cluster Summary 

The summary bar includes the following information about your cluster:

| Item | Description |
|---|---|
| Cloud Provider | The logo of the cloud provider on which the cluster is running. |
| Credential | The name of the credential used to create the cluster. |
| Status | Current status. When a cluster is healthy, the status is *Running*. |
| Nodes | The current number of cluster nodes, including the master node. |
| Uptime | The amount of time (HH:MM) that the cluster has been in the running state since it was started. Each time you stop and restart the cluster, the running time is reset to 0. |
| Created | The date when the cluster was created. The date format is Mon DD, YYYY. For exampple: Oct 27, 2017. |

#### Cluster Information 

| Item | Description |
|---|---|
| Network | The name of the network in which the cluster is running. |
| Subnet | The name of the subnet in which the cluster is running. |
| Cluster User | The name of the cluster user that you created when creating the cluster. |  
| SSH Username | The SSH user which you must use when accessing cluster VMs via SSH. The SSH user is always "cloudbreak". |
| Ambari URL | Link to the Ambari web UI. |
| Region | The region in which the cluster is running in the cloud provider infrastructure. |
| Availability Zone | The availability zone within the region in which the cluster is running. |
| Blueprint | The name of the blueprint selected under "Cluster Type" to create this cluster. |
| Started With | The version of Cloubdreak used to create this cluster. |
| Ambari Version | The Ambari version which this cluster is currently running. |
| HDP Version | The HDP version which this cluster is currently running. |

[comment]: <> (Why is the Ambari link labeled "Remote Access"?)

[comment]: <> (Regarding Ambari and HDP version, if I upgrade, should this show the current version or the original version?)

#### Hardware

This section includes information about your cluster nodes: instance names, instance IDs (with links to the cloud provider console), and public IPs.


#### Tags 

This section lists user-defined tags, in the same order as you added them.

#### Recipes

This section lists recipes executed on the host groups of the cluster.

#### Repository Details 

This sections includes Ambari and HDP repository information. 

#### Event History 

The Event History section shows you events logged for the cluster, with the most recent event at the top. For example, after your cluster has been created, the following messages will be written to the log:

<pre>
Ambari cluster built; Ambari ip:34.215.103.66
10/26/2017, 9:41:58 AM
Building Ambari cluster; Ambari ip:34.215.103.66
10/26/2017, 9:30:20 AM
Starting Ambari cluster services
10/26/2017, 9:27:12 AM
Setting up infrastructure metadata
10/26/2017, 9:27:11 AM
Bootstrapping infrastructure cluster
10/26/2017, 9:26:38 AM
Infrastructure successfully provisioned
10/26/2017, 9:26:37 AM
Billing started, Infrastructure successfully provisioned
10/26/2017, 9:26:37 AM
Infrastructure metadata collection finished
10/26/2017, 9:25:39 AM
Infrastructure creation took 194 seconds
10/26/2017, 9:25:37 AM
Creating infrastructure
10/26/2017, 9:22:22 AM
Setting up HDP image
10/26/2017, 9:22:21 AM</pre>


### Access Cluster via SSH

If you plan to access the cluster via the command line clients, SSH into the master node instance in the cluster. 

* In order to use SSH, you must generate an SSH key pair or use an existing SSH key pair.  
* You can find the cluster instance public IP addresses on the cluster details page.  
* When accessing instances via SSH use the `cloudbreak` user. 

On Mac OS, you can use the following syntax to SSH to the VM:
<pre>ssh -i "privatekey.pem" cloudbreak@publicIP</pre>
For example:
<pre>ssh -i "dominika-kp.pem" cloudbreak@p52.25.169.132</pre>

On Windows, you can SSH using an SSH client such as PuTTY.

### Access Ambari

You can access Ambari web UI by clicking on the links provided in the **Cluster Information** > **Ambari URL**.

**Steps**

1. From the cluster dashboard, click on the tile representing your cluster to navigate to cluster details.

2. Find the Ambari URL in the **Cluster Information** section. This URL is available once the Ambari cluster creation process has completed.  

3. Click on the **Ambari URL** link.

4. The first time you access the server, your browser will attempt to confirm that the SSL Certificate is valid. Since Cloudbreak automatically generates a self-signed certificate, your browser will warn you about an Untrusted Connection and ask you to confirm a Security Exception. Depending on your browser, perform the steps below to proceed.

    | Browser	| Steps |
|---|---|
| Firefox | Click **Advanced** > Click **Add Exception...** > Click **Confirm Security Exception** |
| Safari	| Click **Continue** |
| Chrome |	Click **Advanced** > Click **Proceed...** |


###  User Accounts

The following table describes what credentials to use to access Cloudbreak and Cloudbreak-managed clusters:

| Component | Method | Description |
|---|---|---|
| Cloudbreak | Web UI, CLI | Access with the username and password provided when launching Cloudbreak on the cloud provider. |
| Cloudbreak | SSH to VM | Access as the "cloudbreak" user with the SSH key provided when launching Cloudbreak on the cloud provider. |
| Cluster | SSH to VMs | Access as the "cloudbreak" user with the SSH key provided during cluster creation. |
| Cluster | Ambari UI |Access with the credentials provided in the “Cluster User” parameter during cluster creation. |

