## Access Hive via JDBC

Hive can be accessed via JDBC through the [gateway](gateway.md) that is automatically installed and configured in your cluster. If your cluster configuration includes Hive LLAP, then Hive LLAP is configured with the gateway; otherwise, HiveServer2 is configured. In either case, the transport mode is “http” and the gateway path to Hive is `"${CLUSTER_NAME}/${TOPOLOGY_NAME}/hive"` (for example "test-cluster/db-proxy/hive").

Before you can start using Hive JDBC, you must download the SSL certificate to your truststore. After downloading the SSL certificate, the Hive JDBC endpoint is:

	jdbc:hive2://{GATEWAY_HOST}:8443/;ssl=true;sslTrustStore=gateway.jks;trustStorePassword={GATEWAY_JKS_PASSWORD};transportMode=http;httpPath={CLUSTER_NAME}/{TOPOLOGY_NAME}/hive
	   


### Download SSL Certificate 

By default, the gateway has been configured with a *self-signed certificate* to protect the Hive endpoint via SSL. Therefore, in order to use Hive via JDBC or Beeline client, you *must download the SSL certificate* from the [gateway](gateway.md) and add it to your truststore.

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


Here are two examples of using tools to connect to Hive via JDBC:

- [SQL Workbench/J](#example-sql-workbenchj)  
- [Tableau](#example-tableau)  

### Examples

#### Example: SQL Workbench/J    

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

    <a href="../images/cb_sql-workbench01.png" target="_blank" title="click to enlarge"><img src="../images/cb_sql-workbench01.png" width="650" title="SQL Workbench Configuration"></a> 

10. Click **OK** to save the driver.  

**Step 2: Create a Connection to Hive**

1. From the **Select Connection Profile** window, select the “Hortonworks Hive JDBC" from the **Driver** dropdown.  
2. For **URL** , enter the URL to the cluster instance where gateway is instlaled, such as `jdbc:hive2://52.52.98.57:8443/` (where `52.52.98.57` is the public hostname of your gateway node).  
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
    
    <a href="../images/cb_sql-workbench02.png" target="_blank" title="click to enlarge"><img src="../images/cb_sql-workbench02.png" width="650" title="SQL Workbench Configuration"></a> 

5. Click **OK** to save the properties.
6. Click **Test** to confirm a connection can be established.
7. Click **OK** to make the connection and start using SQL Workbench to query Hive.


#### Example: Tableau

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

    <a href="../images/cb_tableau01.png" target="_blank" title="click to enlarge"><img src="../images/cb_tableau01.png" width="375" title="Tableau Configuration"></a> 

6. Click **OK**.  

    After performing these steps, your configuration should look similar to:
    
    <a href="../images/cb_tableau02.png" target="_blank" title="click to enlarge"><img src="../images/cb_tableau02.png" width="375" title="Tableau Configuration"></a> 
    
6. Click **Sign In** and you will be connected to Hive.  
   
    
