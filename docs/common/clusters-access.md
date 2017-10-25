## Accessing Your Cluster

The following section describes how to access the various services in the cluster.


### Finding Cluster Details

Once your cluster is up and running, click on the tile representing your cluster in the Cloudbreak UI to find information about the cluster. 

<a href="../images/cb-ui-clinfo.png" target="_blank" title="click to enlarge"><img src="../images/cb-ui-clinfo.png" width="650" title="Cluster Information"></a> 

The information presented includes:

* [Cluster Summary](#cluster-summary)
* [Cluster Information](#cluster-information) 
* [Hardware](#hardware)
* [Tags](#tags) 
* [Event History](#event-history) 

#### Cluster Summary 


#### Cluster Information 

* Cluster instance public IP addresses
* UI links 

#### Hardware

This section includes information about your cluster nodes: instance names, instance IDs (with links to the EC2 console), public IPs, and SSH connection information (with a copy option).


#### Tags 

This section lists user-defined tags, in the same order as you added them.


#### Event History 

The EVENT HISTORY tab shows you events logged for the cluster, with the most recent event at the top. For example, after your cluster has been created, the following messages will be written to the log:

<pre></pre>


### Access Cluster via SSH

If you plan to access the cluster via the command line clients, SSH into the master node instance in the cluster. 

* In order to use SSH, you must generate an SSH key pair or use an existing SSH keypair.  
* You can find the cluster instance public IP addresses on the cluster details page.  
* When accessing instances via SSH use the `cloudbreak` user. 

On Mac OS, you can use the following syntax to SSH to the VM:
<pre>ssh -i "privatekey.pem" cloudbreak@publicIP</pre>
For example:
<pre>ssh -i "dominika-kp.pem" cloudbreak@p52.25.169.132</pre>

On Windows, you can SSH using an SSH client such as PuTTY. 