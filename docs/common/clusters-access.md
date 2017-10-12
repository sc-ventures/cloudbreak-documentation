## Accessing Your Cluster

The following section describes how to access the various services in the cluster.

### Finding Cluster Details

Once your cluster is up and running, you can find information about the cluster on the cluster details page in the Cloudbreak UI. To access cluster details page, click on the tile representing your cluster in the Cloudbreak UI. The information presented includes:

* Cluster status
* Event history 
* Cluster instance public IP addresses
* UI links 

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



