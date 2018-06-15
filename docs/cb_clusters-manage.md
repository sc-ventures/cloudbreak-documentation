# Accessing and managing clusters 

## Accessing a cluster

The following section describes how to access the various services in the cluster.


### Cloudbreak user accounts

The following table describes what credentials to use to access Cloudbreak and Cloudbreak-managed clusters:

| Component | Method | Description |
|---|---|---|
| Cloudbreak | Web UI, CLI | Access with the username and password provided when launching Cloudbreak on the cloud provider. |
| Cloudbreak | SSH to VM | Access as the "cloudbreak" user with the SSH key provided when launching Cloudbreak on the cloud provider. |
| Cluster | SSH to VMs | Access as the "cloudbreak" user with the SSH key provided during cluster creation. |
| Cluster | Ambari web UI |Access with the credentials provided in the “Cluster User” parameter during cluster creation. |
| Cluster | Web UIs for specific cluster services | Access with the credentials provided in the “Cluster User” parameter during cluster creation. |


### Finding cluster information in the web UI

Once your cluster is up and running, click on the tile representing your cluster in the Cloudbreak UI to access information related the cluster and access cluster actions. 

<img src="../images/cb_cb-cl-details1.png" width="650" title="Cluster Information"> 

The information presented includes:

* [Cluster summary](#cluster-summary)  
* [Cluster information](#cluster-information)     
* [Event history](#event-history)  


#### Cluster summary 

The summary bar includes the following information about your cluster:

| Item | Description |
|---|---|
| Cluster Name | The name that you selected for your cluster is displayed at the top of the page. Below it is the name of the cluster blueprint. |
| Time Remaining | If you enabled lifetime management for your cluster, the clock next to the cluster name indicates the amount of time that your cluster will run before it gets terminated. Note that the time remaining counter does not stop when you stop the cluster. |
| Cloud Provider | The logo of the cloud provider on which the cluster is running. |
| Credential | The name of the credential used to create the cluster. |
| Status | Current status. When a cluster is healthy, the status is *Running*. |
| Nodes | The current number of cluster nodes, including the master node. |
| Uptime | The amount of time (HH:MM) that the cluster has been in the running state since it was started. Each time you stop and restart the cluster, the running time is reset to 0. |
| Created | The date when the cluster was created. The date format is Mon DD, YYYY. For example: Oct 27, 2017. |

#### Cluster information 

The following information is available on the cluster details page: 

| Item | Description |
|---|---|
| Cluster User | The name of the cluster user that you created when creating the cluster. |  
| SSH Username | The SSH user which you must use when accessing cluster VMs via SSH. The SSH user is always "cloudbreak". |
| Ambari URL | Link to the Ambari web UI. |
| Region | The region in which the cluster is running in the cloud provider infrastructure. |
| Availability Zone | The availability zone within the region in which the cluster is running. |
| Blueprint | The name of the blueprint selected under "Cluster Type" to create this cluster. |
| Created With | The version of Cloudbreak used to create this cluster. |
| Ambari Version | The Ambari version which this cluster is currently running. |
| HDP/HDF Version | The HDP or HDF version which this cluster is currently running. |
| Authentication Source | If you are using an external authentication source (LDAP/AD) for your cluster, you can see it here. Refer to [Using an external authentication source](https://hortonworks.github.io/cloudbreak-documentation/latest/). |


Below this, you will see additional tabs that you can click on in order to see their content:

| Item | Description |
|---|---|
|Hardware | This section includes information about your cluster instances: instance names, instance IDs, instance types, their status, fully qualified domain names (FQDNs), and private and public IPs. |
| Cloud storage | If you configured any cloud storage options, you will see them listed here. |
| Tags | This section lists keys and values of the user-defined tags, in the same order as you added them. |
| Gateway | This section is available when gateway is configured for a cluster. It includes the gateway URL to Ambari and the URLs for the service UIs. |
| Recipes | This section includes recipe-related information. For each recipe, you can see the host group on which a recipe was executed, recipe name, and recipe type. Refer to [Using custom scripts (recipes)](https://hortonworks.github.io/cloudbreak-documentation/latest/). |
| External databases | If you are using an external database for your cluster, you can see it here. Refer to [using an external database](https://hortonworks.github.io/cloudbreak-documentation/latest/). |
| Repository details | This section includes Ambari and HDP/HDF repository information, as you provided it in the "Base Images" section when creating a cluster. |
| Image details | This section includes information about the base image that was used for the Cloudbreak instance. |
| Network | This section includes information about the names of the network and subnet in which the cluster is running and the links to related cloud provider console. |
| Security | This section is only available if you have enabled Kerberos security. It provides you with the details of your Kerberos configuration.  
| Autoscaling | This section includes configuration options related to autoscaling. Refer to [Configuring autoscaling](https://hortonworks.github.io/cloudbreak-documentation/latest/). |



#### Event history 

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


### Accessing cluster via SSH

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


### View cluster blueprints  

Cloudbreak includes a useful option to view blueprints of a future cluster (from the create cluster wizard) or an existing cluster (from cluster details):

* To view a cluster blueprint from the create cluster wizard, on the last page of the wizard, select **Show blueprint**.    

* To view a cluster blueprint for an existing cluster, navigate to cluster details, and from the **ACTIONS** menu, select **Show blueprint**.  


## Managing and monitoring clusters  

You can manage monitor your clusters from the Cloudbreak UI. To do that, click on the tile representing the cluster that you want to access. The actions available for your cluster are listed in the top right corner: 

<img src="../images/cb_cb-cl-details2.png" width="650" title="Cloudbreak web UI"> 


### Retry a cluster

When stack provisioning or cluster creation failure occurs, the "retry" option allows you to resume the process from the last failed step. 

In some cases the cause of a failed stack provisioning or cluster creation can be eliminated by simply retrying the process. For example, in case of a temporary network outage, a retry should be successful. In other cases, a manual modification is required before a retry can succeed. For example, if you are using a custom image but some configuration is missing, causing the process to fail, you must log in to the machine and fix the issue; Only after that you can retry the the process.
 
Only failed stack or cluster creations can be retried. A retry can be initiated any number of times on a failed creation process. 

To retry provisioning a failed stack or cluster, follow these steps.  

**Steps**

1. Browse to the cluster details.

2. Click **Actions** and select **Retry**. 

    Only failed stack or cluster creations can be retried, so the option is only available in these two cases.  

3. Click **Yes** to confirm. 

    The operation continues from the last failed step. 
    

### Resize a cluster

To resize a cluster, follow these steps.

> Cluster resizing is not supported for HDF clusters.  
> To configure automated cluster scaling, refer to [Configure autoscaling](https://hortonworks.github.io/cloudbreak-documentation/latest/).   

**Steps**

1. Browse to the cluster details.

2. Click **Actions** and select **Resize**. The cluster resize dialog is displayed.

3. Using the +/- controls, adjust the number of nodes for a chosen host group. 

    > You can only modify one host group at a time.   
    > It is not possible to resize the Ambari server host group.     

4. Click **Yes** to confirm the scale-up/scale-down.

    While nodes are being added or removed, cluster status changes to "Update In Progress". Once the operation has completed, cluster status changes back to "Running". 


### Synchronize a cluster 

Use the **sync** option if you:  

* Made changes on your cloud provider side (for example, deleted cluster VMs) and you would like to synchronize Cloudbreak with the cloud provider.  
* Manually changed service status in Ambari (for example, restarted services).   

[comment]: <> (What are some examples where this option should be used?)

To synchronize your cluster with the cloud provider, follow these steps. 

**Steps**

1. Browse to the cluster details.

2. Click **Actions** and select **Sync**.
 
3. Click **Yes** to confirm.

    Your cluster infrastructure is synchronized based on changes on the cloud provider. The updates are written to the "Event History". 
 
 
### Stop a cluster 

Cloudbreak supports stopping and restarting clusters. To stop and restart a cluster managed by Cloudbreak, use the options available from the Cloudbreak UI. 

**Steps**

1. Browse to the cluster details.
 
2. Click **Stop** to stop a currently running cluster.  

3. Click **Yes** to confirm. 

4. Your cluster status changes to "Stopping in progress" and then to "Stopped". Once stopping the infrastructure has completed, you will see a **Start** option to restart your cluster. 

When a cluster is in the "stopped" state, you are not charged for the VMs, but you are charged for external storage.  



### Restart a cluster 

If your cluster is in the "Stopped" state, you can restart the cluster by follow these steps.

**Steps**

1. click **Start**. This option is only available when the cluster has been stopped. 

2. Click **Yes** to confirm.

    Your cluster status changes to "Start in progress" and then to "Running". 



### Terminate a cluster 

To terminate a cluster managed by Cloudbreak, use the option available from the Cloudbreak UI. 

**Steps**

1. Browse to the cluster details.
 
2. Click **Terminate**. 

3. Click **Yes** to confirm.

    All cluster-related resources will be deleted, unless the resources (such as networks and subnets) existed prior to cluster creation or are used by other VMs in which case they will be preserved. 


### Force terminate a cluster 

Cluster deletion may fail if Cloudbreak is unable to delete one or more of the cloud resources that were part of your cluster infrastructure. In such as case, you can use the **Terminate** > **Force terminate** option to remove the cluster entry from the Cloudbreak web UI, but you must also check your cloud provider account to see if there are any resources that must be deleted manually.

**Steps**

1. Browse to the cluster details.
 
2. Click **Terminate**. 

3. Check  **Force terminate**.

3. Click **Yes** to confirm. 

    When terminating a cluster with Kerberos enabled, you have an option to disable Kerberos prior to cluster termination. This option removes any cluster-related principals from the KDC.

4. This deletes the cluster tile from the UI.  

4. Log in to your cloud provider account and [manually delete](https://hortonworks.github.io/cloudbreak-documentation/latest/) any resources that failed to be deleted.



### View cluster history

From the navigation menu in the Cloudbreak UI, you can access the History page that allows you to generate a report showing basic information related to the clusters that were running within the specified range of dates.

To generate a report, follow these steps.

**Steps**

1. From the Cloudbreak UI navigation menu, select **History**.

2. On the History page, select the range of dates and click **Show History** to generate a tabular report for the selected period.


#### History report content 

Each entry in the report represents one cluster instance group. For each entry, the report includes the following information:

* **Created** - The date when your cluster was created (YYYY-MM-DD).
* **Provider** - The name of the cloud provider (AWS, Azure, Google, or OpenStack) on which the cluster instances are/were running.
* **Cluster Name** - The name that you selected for the cluster.  
* **Instance Group** - The name of the host group.   
* **Instance Count** - The number of nodes in the host group. This number may be a decimal if a cluster has been resized.
* **Instance Type** - Provider-specific VM type of the cluster instances. 
* **Region** - The AWS region in which your cluster is/was running.
* **Availability Zone** - The availability zone in which your cluster is/was running.      
* **Running Time (hours)** - The sum of the running times for all the nodes in the instance group.

The **AGGREGATE RUNNING TIME** is the sum of the Running Times, adjusted for the selected time range.

To learn about how your cloud provider bills you for the VMs, refer to their documentation:

* [AWS](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-instance-hour-billing/)      
* [Azure](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/linux/)     
* [GCP](https://cloud.google.com/compute/pricing)   



## Access Hive via JDBC

Hive can be accessed via JDBC through the [gateway](https://hortonworks.github.io/cloudbreak-documentation/latest/) that is automatically installed and configured in your cluster. If your cluster configuration includes Hive LLAP, then Hive LLAP is configured with the gateway; otherwise, HiveServer2 is configured. In either case, the transport mode is “http” and the gateway path to Hive is `"${CLUSTER_NAME}/${TOPOLOGY_NAME}/hive"` (for example "test-cluster/db-proxy/hive").

Before you can start using Hive JDBC, you must download the SSL certificate to your truststore. After downloading the SSL certificate, the Hive JDBC endpoint is:

	jdbc:hive2://{GATEWAY_HOST}:8443/;ssl=true;sslTrustStore=gateway.jks;trustStorePassword={GATEWAY_JKS_PASSWORD};transportMode=http;httpPath={CLUSTER_NAME}/{TOPOLOGY_NAME}/hive
	   


### Download SSL Certificate 

By default, the gateway has been configured with a *self-signed certificate* to protect the Hive endpoint via SSL. Therefore, in order to use Hive via JDBC or Beeline client, you *must download the SSL certificate* from the [gateway](https://hortonworks.github.io/cloudbreak-documentation/latest/) and add it to your truststore.

**Steps** 

On **Linux or OSX**, you can download the self-signed SSL certificate by using the following commands:

    export GATEWAY_HOST=IP_OF_GATEWAY_NODE
    export GATEWAY_JKS_PASSWORD=GATEWAY_PASSWORD
    openssl s_client -servername ${GATEWAY_HOST} -connect ${GATEWAY_HOST}:8443 -showcerts </dev/null | openssl x509 -outform PEM > gateway.pem
    keytool -import -alias gateway-identity -file gateway.pem -keystore gateway.jks -storepass ${GATEWAY_JKS_PASSWORD}

<small>Where:<br>
GATEWAY_HOST - Set this to the IP address of the instance on which gateway is running (Ambari server node).<br>
GATEWAY_JKS_PASSWORD - Create a password for the truststore that will hold the *self-signed certificate*. The password must be at least 6 characters long.</small>

For example:

    export GATEWAY_HOST=2-52-86-252-73
    export GATEWAY_JKS_PASSWORD=Hadoop123!
    openssl s_client -servername ${GATEWAY_HOST} -connect ${GATEWAY_HOST}:8443 -showcerts </dev/null | openssl x509 -outform PEM > gateway.pem
    keytool -import -alias gateway-identity -file gateway.pem -keystore gateway.jks -storepass ${GATEWAY_JKS_PASSWORD}
    
After executing these commands, **gateway.pem** and **gateway.jks** files will be downloaded onto your computer to the location where you ran the commands.


### Example: SQL Workbench/J    

[SQL Workbench/J](http://www.sql-workbench.net/) is a cross-platform SQL tool that can be used to access database systems. In this example, we will provide a high-level overview of the steps required to setup SQL Workbench to access Hive via JDBC.

**Prerequisite:** [Download the SSL Certificate](#download-ssl-certificate) 

**Step 1: Install SQL Workbench and Hive JDBC Driver**

1. Download and install *SQL Workbench*. Refer to <a href="http://www.sql-workbench.net/getting-started.html" target="_blank">http://www.sql-workbench.net/getting-started.html</a>. 
2. Download the *Hortonworks JDBC Driver for Apache Hive* from <a href="https://hortonworks.com/downloads/#addons" target="_blank">https://hortonworks.com/downloads/#addons</a>. Next, extract the driver package.  

**Step 2: Configure SQL Workbench with Hive JDBC Driver**

4. Launch SQL Workbench. 
5. The **Select Connection Profile** window should be open by default. If it is not, you can open it from **File** > **Connect window**.  
6.  Click **Manage Drivers**. The **Manage drivers** window will appear.
6. Click <img src="../images/cb_sql-workbench-wb1.png" width="30"> to create a new driver, and enter the **Name**: “Hortonworks Hive JDBC”.
7. Click <img src="../images/cb_sql-workbench-wb2.png"  width="35"> and then browse to the Hortonworks JDBC Driver for Apache Hive package that you downloaded earlier. Next, select the JDBC Driver JAR files in the package.
8. When prompted, select the “com.simba.hive.jdbc41.HS2Driver” driver.  
9. For the **Sample URL**, enter: `jdbc:hive2://${GATEWAY_HOST}:8443/`  
10. After performing these steps, your configuration should look similar to: 

    <img src="../images/cb_sql-workbench01.png" width="650" title="SQL Workbench Configuration"></a> 

10. Click **OK** to save the driver.  

**Step 2: Create a Connection to Hive**

1. From the **Select Connection Profile** window, select the “Hortonworks Hive JDBC" from the **Driver** dropdown.  
2. For **URL** , enter the URL to the cluster instance where gateway is installed, such as `jdbc:hive2://52.52.98.57:8443/` (where `52.52.98.57` is the public hostname of your gateway node).  
3. For **Username** and **Password**, enter the credentials that you created when creating your cluster.
4. Click **Extended Properties** and add the following properties:

    | Property | Value |
    |---|---|
    | ssl | *1* |
    | transportMode | *http* |
    | httpPath | Provide `"${CLUSTER_NAME}/${TOPOLOGY_NAME}/hive"`. For example `hive-test/db-proxy/hive` |
    | sslTrustStore | Enter the path to the **gateway.jks** file. This file was generated when you [downloaded the SSL certificate](#download-ssl-certificate). |
    | trustStorePassword | Enter the GATEWAY_JKS_PASSWORD that you specified when you [downloaded the SSL certificate](#download-ssl-certificate). |
    
    After performing these steps, your configuration should look similar to:
    
    <img src="../images/cb_sql-workbench02.png" width="650" title="SQL Workbench Configuration"></a> 

5. Click **OK** to save the properties.
6. Click **Test** to confirm a connection can be established.
7. Click **OK** to make the connection and start using SQL Workbench to query Hive.


### Example: Tableau

[Tableau](https://www.tableau.com/) is a business intelligence tool for interacting with and visualizing data via SQL. Connecting Tableau to Hive requires the use of an ODBC driver. In this example, we will provide high-level steps required to set up Tableau to access Hive.

**Prerequisite:** [Download the SSL Certificate](#download-ssl-certificate)  

**Step 1: Install ODBC Driver**

1. Download the *Hortonworks ODBC Driver for Apache Hive* from <a href="https://hortonworks.com/downloads/#addons" target="_blank">https://hortonworks.com/downloads/#addons</a>.    2. Next, extract and install the driver.  

**Step 2: Launch Tableau and Connect to Hive**

1. Launch Tableau. If you do not already have Tableau, you can download a trial version from <a href="https://www.tableau.com/trial/download-tableau" target="_blank">https://www.tableau.com/trial/download-tableau</a>.
2. In Tableau, create a connection to a “Hortonworks Hadoop Hive” server. Enter the following:
    
    | Property | Value |
    |---|---|
    | Server | Enter the public hostname of your controller node instance. |
    | Port | *8443* |
    | Type | *HiveServer2* |
    | Authentication | *Username and Password* |
    | Transport | *HTTP* |
    | Username | Enter the cluster username created when creating your cluster |
    | Password | Enter the cluster password created when creating your cluster |
    | HTTP Path | Provide `"${CLUSTER_NAME}/${TOPOLOGY_NAME}/hive"`. For example `hive-test/db-proxy/hive` |
    
3. Check the **Require SSL** checkbox.  
4. Click on the text underneath the checkbox to **add a configuration file** link.  
5. Specify to use a custom SSL certificate, and then browse to the SSL certificate **gateway.pem** file that was generated when you [downloaded the SSL certificate](#download-ssl-certificate) as a prerequisite. 

    <img src="../images/cb_tableau01.png" width="375" title="Tableau Configuration"></a> 

6. Click **OK**.  

    After performing these steps, your configuration should look similar to:
    
    <img src="../images/cb_tableau02.png" width="375" title="Tableau Configuration"> 
    
6. Click **Sign In** and you will be connected to Hive.  
   
    


