# Configuring advanced Cloudbreak options 

## Configuring external Cloudbreak database

By default, Cloudbreak uses an embedded PostgreSQL database to persist data related to Cloudbreak configuration, setup, and so on. For a production Cloudbreak deployment, you must configure an external database. 

### Supported databases

An embedded PostgreSQL 9.6.1 database is used by Cloudbreak by default. If you would like to
use an external database for Cloudbreak, you may use the following supported database types and versions: 

| Database type | Supported version |
|---|---| 
| External PostgreSQL | 9.6.1 or above |
| External MySQL | Not supported |
| External MariaDB | Not supported |
| External Oracle | Not supported |
| External SQL Server | Not supported |


### Configure external Cloudbreak database

The following section describes how to use Cloudbreak with an existing external database, other than
the embedded PostgreSQL database instance that Cloudbreak uses by default. To configure an external PostgreSQL database for Cloudbreak, perform these steps. 

**Steps**

1. On your Cloudbreak host machine, set the following environment variables according to the settings of your external database: 

    <pre><small>export DATABASE_HOST=my.database.host
export DATABASE_PORT=5432
export DATABASE_USERNAME=admin
export DATABASE_PASSWORD=Admin123!
    </small></pre>
 
2. On your external database, create three databases: `cbdb, uaadb, periscopedb`. You can create these databases using the `createdb` utility with the following commands:
   
    <pre><small>createdb -h $DATABASE_HOST -p $DATABASE_PORT -U $DATABASE_USERNAME cbdb
    createdb -h $DATABASE_HOST -p $DATABASE_PORT -U $DATABASE_USERNAME uaadb
    createdb -h $DATABASE_HOST -p $DATABASE_PORT -U $DATABASE_USERNAME periscopedb</small></pre>
            
    For more information refer to the [PostgreSQL documentation](https://www.postgresql.org/docs/9.6/static/app-createdb.html).   
    Alternatively, you can log in to the management interface of your external database and execute [create database](https://www.postgresql.org/docs/9.6/static/sql-createdatabase.html) commands directly. 
     
     
3. Set the following variables in your Cloudbreak Profile file. Modify the database parameters according to your external database.

    <pre><small>export DATABASE_HOST=my.database.host
    export DATABASE_PORT=5432
    export DATABASE_USERNAME=admin
    export DATABASE_PASSWORD=Admin123!
    
    export CB_DB_PORT_5432_TCP_ADDR=$DATABASE_HOST
    export CB_DB_PORT_5432_TCP_PORT=$DATABASE_PORT
    export CB_DB_ENV_USER=$DATABASE_USERNAME
    export CB_DB_ENV_PASS=$DATABASE_PASSWORD
    export CB_DB_ENV_DB=cbdb
    
    export PERISCOPE_DB_TCP_ADDR=$DATABASE_HOST
    export PERISCOPE_DB_TCP_PORT=$DATABASE_PORT
    export PERISCOPE_DB_USER=$DATABASE_USERNAME
    export PERISCOPE_DB_PASS=$DATABASE_PASSWORD
    export PERISCOPE_DB_NAME=periscopedb
    export PERISCOPE_DB_SCHEMA_NAME=public
    
    export IDENTITY_DB_URL=$DATABASE_HOST:$DATABASE_PORT
    export IDENTITY_DB_USER=$DATABASE_USERNAME
    export IDENTITY_DB_PASS=$DATABASE_PASSWORD
    export IDENTITY_DB_NAME=uaadb</small></pre>
    

3. Restart Cloudbreak application by using the `cbd restart` command. 

After performing these steps, your external database will be used for Cloudbreak instead of the built-in database. 

> If you want to migrate your existing data (such as  blueprints, recipes, and so on) from the embedded database to the external one, then after completing these steps, you should also create a backup of your original database and then restore it in the external database.   		



## Restrict inbound access to clusters 

We recommend that after launching Cloudbreak you set CB_DEFAULT_GATEWAY_CIDR in your Cloudbreak's Profile file.
When you launch a cluster, and Cloudbreak proposes security groups, this CIDR will be used
for the Cloudbreak to the Cluster master node (i.e. the host with Ambari Server) with this IP. This limits access
from Cloudbreak to this cluster name for ports 9443 and 22 for Cloudbreak communication and management of the cluster.

**Steps** 

1. Set CB_DEFAULT_GATEWAY_CIDR to the CIDR address range which is used by Cloudbreak to communicate with the cluster:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32</pre>
    
    Or, if your Cloudbreak communicates with the cluster through multiple addresses, set multiple addresses separated with a comma:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32,18.17.16.15/32</pre>
    
2. If Cloudbreak has already been started, restart it using `cbd restart`.     
    
3. When CB_DEFAULT_GATEWAY_CIDR is set, two additional rules are added to your Ambari node security group: (1) port 9443 open to your Cloudbreak IP, and (2) port 22 open to your Cloudbreak IP. You can view and edit these default rules in the create cluster wizard. 


## Add SSL certificate for Cloudbreak web UI 

By default Cloudbreak has been configured with a self-signed certificate for access via HTTPS. This is sufficient for many deployments such as trials, development, testing, or staging. However, for production deployments, a trusted certificate is preferred and can be configured in the controller. Follow these steps to configure the cloud controller to use your own trusted certificate. 

**Prerequisites**

To use your own certificate, you must have:

* A resolvable fully qualified domain name (FQDN) for the controller host IP address. For example, this can be set up in [Amazon Route 53](https://aws.amazon.com/documentation/route53/).  
* A valid SSL certificate for this fully qualified domain name. The certificate can be obtained from a number of certificate providers.  

**Steps**

1. SSH to the Cloudbreak host instance:

    <pre><small>ssh -i mykeypair.pem cloudbreak@[CONTROLLER-IP-ADDRESS]</small></pre>
    
2. Make sure that the target fully qualified domain name (FQDN) which you plan to use for Cloudbreak is resolvable:

    <pre><small>nslookup [TARGET-CONTROLLER-FQDN]</small></pre>
    
    For example:

    <pre><small>nslookup hdcloud.example.com</small></pre>
    
3. Browse to the Cloudbreak deployment directory and edit the `Profile` file:

    <pre><small>vi /var/lib/cloudbreak-deployment/Profile</small></pre>
    
4. Replace the value of the `PUBLIC_IP` variable with the `TARGET-CONTROLLER-FQDN` value:

    <pre><small>PUBLIC_IP=[TARGET-CONTROLLER-FQDN]</small></pre>
    
5. Copy your private key and certificate files for the FQDN onto the Cloudbreak host. These files must be placed under `/var/lib/cloudbreak-deployment/certs/traefik/` directory.

    > File permissions for the private key and certificate files can be set to 600.

    | File | Example |
|---|---|
| PRIV-KEY-LOCATION	| /var/lib/cloudbreak-deployment/certs/traefik/hdcloud.example.com.key |
| CERT-LOCATION	| /var/lib/cloudbreak-deployment/certs/traefik/hdcloud.example.com.crt |

6. Configure TLS details in your `Profile` by adding the following line at the end of the file.

    > Notice that `CERT-LOCATION` and `PRIV-KEY-LOCATION` are file locations from Step 5, starting at the `/certs/...` path.

    <pre><small>export CBD_TRAEFIK_TLS=”[CERT-LOCATION],[PRIV-KEY-LOCATION]”</small></pre>
    
    For example:

    <pre><small>export CBD_TRAEFIK_TLS="/certs/traefik/hdcloud.example.com.crt,/certs/traefik/hdcloud.example.com.key"</small></pre>
    
7. Restart Cloudbreak deployer:

    <pre><small>cbd restart</small></pre>
    
8. Using your web browser, access the Cloudbreak UI using the new resolvable fully qualified domain name.

9. Confirm that the connection is SSL-protected and that the certificate used is the certificate that you provided to Cloudbreak.


## Moving a Cloudbreak instance

To transfer a Cloudbreak instance from one host to another, perform these tasks:

1. If you are using the embedded PostgreSQL database, [back up current Cloudbreak database](#back-up-cloudbreak-database) data  
2. Launch a new Cloudbreak instance and start Cloudbreak. Refer to [Launch Cloudbreak](https://hortonworks.github.io/cloudbreak-documentation/latest/)   
3. If you are using the embedded PostgreSQL database, [populate the new Cloudbreak instance database with the dump from the original Cloudbreak instance](#populate-database-with-dump-from-original-cloudbreak-instance) on the new host.  
4. [Modify Cloudbreak Profile](#modify-cloudbreak-profile)  


### Back up Cloudbreak database 

To create a backup of the embedded PostgreSQL database, perform these steps.

**Steps** 

1. On your Cloudbreak host machine, execute the following  command to enter the container of the database:

    <pre>docker exec -it cbreak_commondb_1 bash</pre> 
    If it is not running, start the database container by using the `docker start cbreak_commondb_1` command.

3. Create three database dumps (cbdb, uaadb, periscopedb):  

    <pre><small>pg_dump -Fc -U postgres cbdb > cbdb.dump
    pg_dump -Fc -U postgres uaadb > uaadb.dump
    pg_dump -Fc -U postgres periscopedb > periscopedb.dump</small></pre>
                
4. Quit from the container with shortcut `CTRL+d`.

5. Save the previously created dumps to the host instance:               

    <pre><small>docker cp cbreak_commondb_1:/cbdb.dump ./cbdb.dump
    docker cp cbreak_commondb_1:/uaadb.dump ./uaadb.dump
    docker cp cbreak_commondb_1:/periscopedb.dump ./periscopedb.dump</small></pre>



### Populate database with dump from original Cloudbreak instance

Perform these steps to populate databases with information from the Cloudbreak server.

**Steps** 

1. Copy the saved database files from [Back up Cloudbreak database](#back-up-cloudbreak-database) to the new Cloudbreak server host.

2. Copy the dump files into the database container with the following commands. Modify the location as necessary (The example below assumes that the files are in `/tmp`):

    <pre><small>docker cp /tmp/cbdb.dump cbreak_commondb_1:/cbdb.dump
    docker cp /tmp/uaadb.dump cbreak_commondb_1:/uaadb.dump
    docker cp /tmp/periscopedb.dump cbreak_commondb_1:/periscopedb.dump</small></pre>
   
3. Execute the following command to stop the container:

    <pre>docker stop cbreak_identity_1</pre>

4. Execute the following command to enter the container of the database:

    <pre>docker exec -it cbreak_commondb_1 bash</pre>

5. Execute the following commands:
   
    <pre><small>psql -U postgres
    drop database uaadb;
    drop database cbdb;
    drop database periscopedb;
    create database uaadb;
    create database cbdb;
    create database periscopedb;</small></pre>
 
    > If you get `ERROR:  database "uaadb" is being accessed by other users` error, ensure that    cbreak_identity_1 container is not running and then retry dropping uaadb.  

5. Exit the PostgreSQL interactive terminal.
    <pre><small>\q</small></pre> 
     
6. Restore the databases from the original backups:
   
    <pre><small>pg_restore -U postgres -d periscopedb periscopedb.dump
    pg_restore -U postgres -d cbdb cbdb.dump
    pg_restore -U postgres -d uaadb uaadb.dump</small></pre>
   
7. Quit from the container with the shortcut `CTRL+d`.     


### Modify Cloudbreak Profile

Perform these steps to ensure that your new Profile file is correctly set up. 

**Steps** 

1. Ensure that the following parameter values match in the origin and target Profile files and modify Profile file of the target environment if necessary:

    <pre><small>export UAA_DEFAULT_USER_EMAIL=admin@example.com
    export UAA_DEFAULT_SECRET=cbsecret
    export UAA_DEFAULT_USER_PW=cbuser</small></pre>
    
2. Restart Cloudbreak application by using the `cbd restart` command.  
 
    After performing these steps the migration is complete. To verify, log in to the UI of your new Cloudbreak instance and make sure that it contains the information from your old instance.  


## Configuring Cloudbreak for LDAP/AD authentication    

By default Cloudbreak uses an internal system as the user store for authentication (enabled by using [CloudFoundry UAA](https://github.com/cloudfoundry/uaa )). If you would like to configure LDAP or Active Directory (AD) external authentication, you need to:  

1. Collect the [following information](#ldapad-information) about your LDAP/AD setup    
2. [Configure Cloudbreak](#configuring-cloudbreak-for-ldapad) to work with that LDAP/AD setup


### LDAP/AD information 

The following table details the properties and values that you need to know about your LDAP/AD environment on order to use the LDAP/AD with Cloudbreak: 

| Parameter | Description | Example |
|---|---|---|
| **base** |
| url | The LDAP url with port | `ldap://10.0.3.128:389/` | 
| userDn | Enter the root Distinguished Name to search in the directory for users. | `cn=Administrator,ou=srv,dc=hortonworks,dc=local` |
| password | Enter your root Distinguished Name password. |  `MyPassword1234!`|
| searchBase | Enter your LDAP user search base. This defines the location in the directory from which the LDAP search begins. | `ou=Users,dc=hortonworks,dc=local` |
| searchFilter | Enter the attribute for which to conduct a search on the user base. | `mail={0}` |
| **groups** |
| searchBase | Enter your LDAP group search base. This defines the location in the directory from which the LDAP search begins. | `ou=Groups,dc=hortonworks,dc=local` |
| groupSearchFilter| Enter the attribute for which to conduct a search on the group base. | `member={0}` |

### Configuring Cloudbreak for LDAP/AD

There are two parts to configuring Cloudbreak for LDAP/AD:

* Configuring LDAP/AD user authentication for Cloudbreak  
* Configuring LDAP/AD group authorization for Cloudbreak  

#### Configure user authentication

Configure LDAP/AD user authentication for Cloudbreak by using these steps. 

**Steps** 

1. On the Cloudbreak host, browse to `/var/lib/cloudbreak-deployment`.  
2. Create a new file called `uaa-changes.yml`. 

    > The name of this file can be customized by setting the following in Profile: `export UAA_SETTINGS_FILE=myldap.yml`
    
3. In the yml file enter the following using your [LDAP/AD information](#ldapad-information). Next, save the file and restart Cloudbreak.  

<pre>spring_profiles: postgresql,ldap

ldap:
  profile:
    file: ldap/ldap-search-and-bind.xml
  base:
    url: ldap://10.0.3.138:389
    userDn: cn=Administrator,ou=srv,dc=hortonworks,dc=local
    password: ’mypassword’
    searchBase: ou=Users,dc=hortonworks,dc=local
    searchFilter: mail={0}
  groups:
    file: ldap/ldap-groups-map-to-scopes.xml
    searchBase: ou=Groups,dc=hortonworks,dc=local
    searchSubtree: false
    maxSearchDepth: 1
    groupSearchFilter: member={0}
    autoAdd: true
</pre>



#### Configure group authorization 

Once user authentication is configured, you need to configure which group(s) can access Cloudbreak. Users (once authenticated) will be granted permission to access Cloudbreak and use the capabilities of Cloudbreak based on their group member. The following describes how to create (i.e. execute-and-map) a group authorization and how to remove (i.e. delete-mapping) an authorization. 

To create a group authorization, execute the following (for example: to add “Analysts” group):
 
<pre>cbd util execute-ldap-mapping cn=Analysts,ou=Groups,dc=hortonworks,dc=local</pre>

To remove a group authorization, execute the following (for example: to remove “Analysts” group):

<pre>cbd util delete-ldap-mapping cn=Analysts,ou=Groups,dc=hortonworks,dc=local</pre>


## Configuring outbound internet access and proxy  

Depending on your enterprise requirements, you may have limited or restricted outbound network access and/or require the use of an internet proxy. Installing and configuring Cloudbreak, as well as creating cloud resources and clusters on those resources requires outbound network access to certain destinations, and in some cases must go through a proxy.

This section provides information on the outbound network destinations for Cloudbreak, and instructions on how to configure Cloudbreak to use a proxy for outbound access (if required).

| Scenario | Documentation |
|---|---|
| My environment has limited outbound internet access | Refer to [Outbound network access destinations](#outbound-network-access-destinations) for information on network rules. |
| My environment requires use of a proxy for outbound internet access | Refer to [Using a proxy](#using-a-proxy) for information on using a proxy with Cloudbreak. | 


### Outbound network access destinations

To install and configure Cloudbreak, you will need the following outbound destinations available:

<table>
<tr>
    <th>Destination</th>
    <th>Description</th> 
  </tr>
  <tr>
    <td>*.docker.io</td>
    <td>Obtain the Docker images for Cloudbreak.</td> 
  </tr>
  <tr>
    <td><p>raw.githubusercontent.com</p><p>github.com</p><p>s3.amazonaws.com</p><p>*.cloudfront.net</p></td>
    <td>Obtain Cloudbreak dependencies.</td> 
  </tr>
  <tr>
  <td>cloudbreak-imagecatalog.s3.amazonaws.com </td><td> The default Cloudbreak image catalog used for VMs. Refer to <a href="../images/index.html">Custom images</a> for more information on image catalogs. </td>
  </tr>
</table>

Once Cloudbreak is installed and configured, you will need the following outbound destinations available in order to communicate with the cloud provider APIs to obtain cloud resources for clusters.

<table>
<tr>
    <th>Cloud provider</th>
    <th>Cloud provider API destinations</th> 
  </tr>
  <tr>
  <td>Amazon Web Services</td>
  <td>*.amazonaws.com</td>
 </tr>
  <td> Microsoft Azure </td>
  <td> <p>*.microsoftonline.com</p><p>*.windows.net</p><p>*.azure.com</p></td>
   </tr>
   <tr>
  <td> Google Cloud Platform  </td>  
  <td> <p>accounts.google.com</p><p>*.googleapis.com</p></td>
  </tr>
</table>

To install the cluster software, you can: 

a) use the public hosted repositories provided by Hortonworks, or  
b) specify your own local hosted repositories when you create a cluster. 

If you choose to (a) use the public hosted repositories, be sure to allow outbound access to the following destinations:

* private-repo-1.hortonworks.com  
* public-repo-1.hortonworks.com  


### Using a proxy 

In some cases, your environment requires all internet traffic to go through an internet proxy. This section describes the following:

* How to [set up Cloudbreak to use a proxy](#set-up-cloudbreak-to-use-a-proxy)  
* How to [configure your cluster hosts to use a proxy](#set-up-clusters-to-use-a-proxy)  


#### Set up Cloudbreak to use a proxy 

Use these steps if you would like to set up Cloudbreak to use your proxy. 

**Steps**

1. After downloading and installing Cloudbreak, configure the Docker daemon to use proxy by adding the following to the Docker service file:

    <pre>Environment="HTTP_PROXY=http://my-proxy-host:my-proxy-port" "NO_PROXY=localhost,127.0.0.1"</pre>   
    
    For example:
    
    <pre>vi /etc/systemd/system/docker.service -> Environment="HTTP_PROXY=http://10.0.2.237:3128" "NO_PROXY=localhost,127.0.0.1"</pre>

    For more information refer to
[Docker docs](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy).     

2. Ensure that ports 9443 and 8443 are handled as SSL connections in the proxy config.   

3. Configure proxy settings in the Profile file by setting the following variables:  

<pre>HTTP_PROXY_HOST=your-proxy-host
HTTPS_PROXY_HOST=your-proxy-host
PROXY_PORT=your-proxy-port
PROXY_USER=your-proxy-user
PROXY_PASSWORD=your-proxy-password
#NON_PROXY_HOSTS
#HTTPS_PROXYFORCLUSTERCONNECTION=false</pre>

For example:

<pre>HTTP_PROXY_HOST=10.0.2.237
HTTPS_PROXY_HOST=10.0.2.237
PROXY_PORT=3128
PROXY_USER=squid
PROXY_PASSWORD=squid
#NON_PROXY_HOSTS
#HTTPS_PROXYFORCLUSTERCONNECTION=false</pre>


#### Set up clusters to use a proxy 

Use the following guidelines to find out what steps to perform in order to set up your clusters to use a proxy:  

| What base image are you using?| Where are the platform repositories? | What to do | 
|---|---|---|
| Default | Public | Use [Register a Proxy](https://hortonworks.github.io/cloudbreak-documentation/latest/) |
| Default | Local | Use [Register a Proxy](https://hortonworks.github.io/cloudbreak-documentation/latest/)|
| Custom | Public | Set up the proxy on your custom image OR use [Register a Proxy](https://hortonworks.github.io/cloudbreak-documentation/latest/).|
| Custom | Local | Not required. Skip this section. |

You can define a proxy configuration as an external source in Cloudbreak web UI or CLI, and then (optionally) specify to configure that proxy configuration on the hosts that are part of the cluster during cluster create. Refer to [Register a Proxy](https://hortonworks.github.io/cloudbreak-documentation/latest/) for more information.  


#### Advanced proxy setup scenarios 

In some cases, Cloudbreak using the proxy might vary depending on your Cloudbreak -> cluster deployment. This section describes two scenarios:

* **Scenario 1**: Cloudbreak needs to go through a proxy to access the Cloud provider APIs (and other public internet resources) but can talk to the cluster hosts directly.  
* **Scenario 2**: Cloudbreak needs to go through a proxy to access the Cloud provider APIs (and other public internet resources) and the cluster hosts.  


**Scenario 1**

In this scenario, Cloudbreak can resolve and communicate with the Ambari Server in the cluster hosts directly. For example, this can be a scenario where Cloudbreak is deployed in the same VPC/VNet as the clusters and will not go through the proxy. However, Cloudbreak will communicate to the public Cloud Provider APIs via the proxy.

To configure this scenario, set this setting in your Profile file:

<pre>HTTPS_PROXYFORCLUSTERCONNECTION = false</pre>

<img src="../images/cb_cb-proxy1.png" width="650" title="GCP Console">

**Scenario 2**

In this scenario, Cloudbreak will connect to the Ambari Server through the configured proxy. For example, this can be a scenario where Cloudbreak is deployed to a different VPC/VNet than the cluster and must go through a proxy. Communication to the public cloud provider APIs also is via the proxy.

To configure this scenario, set this setting in your Profile file:

<pre>HTTPS_PROXYFORCLUSTERCONNECTION = true</pre>

<img src="../images/cb_cb-proxy2.png" width="650" title="GCP Console">



## Configure access from custom domains

Cloudbreak deployer, which uses UAA as an identity provider, supports multitenancy. In UAA, multitenancy is managed through identity zones. An identity zone is accessed through a unique subdomain. For example, if the standard UAA responds to `https://uaa.10.244.0.34.xip.io`, a zone on this UAA can be accessed through a unique subdomain `https://testzone1.uaa.10.244.0.34.xip.io`.

If you want to use a custom domain for your identity or deployment, add the `UAA_ZONE_DOMAIN` line to your `Profile`:

<pre>export UAA_ZONE_DOMAIN=my-subdomain.example.com</pre>

This variable is necessary for UAA to identify which zone provider should handle the requests that arrive to that domain.




## Change default Cloudbreak ports  

By default, Cloudbreak uses ports 80 (HTTP) and 443 (HTTPS) to access the Cloudbreak server (for the web UI and for the CLI). To change these port numbers, you must edit the Profile file on your Cloudbreak host. 

Cloudbreak should not be running when you change the port numbers. Edit Profile either before you start Cloudbreak the first time or stop Cloudbreak before editing the file.

**Steps**  

1. Navigate to the Cloudbreak deployment directory (typically `/var/lib/cloudbreak-deployment`) and open the Profile file with a text editor. 

2. Add one or both of the following parameters, setting them to the port numbers that you want to use:

    <pre>export PUBLIC_HTTP_PORT=111
export PUBLIC_HTTPS_PORT=222</pre>

3. Start or restart Cloudbreak by using `cbd start` or `cbd restart`. 

4. This change affects Cloudbreak CLI configuration. When [configuring the CLI](https://hortonworks.github.io/cloudbreak-documentation/latest/), you must provide these ports as part of the server URL. For example: 

    <pre>cb configure --server http://cb.server.address:111 --username  test@hortonworks.com
cb configure --server https://cb.server.address:222 --username  test@hortonworks.com</pre>



## Disable providers 

If you are planning to use Cloudbreak with a specific cloud provider or a specific set of cloud providers, you may want to disable the remaining providers. For example, if you are planning to use Cloudbreak with Azure only, you may want to disable AWS, Google Cloud, and OpenStack. 

**Steps**

1. Navigate to the Cloudbreak deployment directory and edit Profile. For example:

    <pre>cd /var/lib/cloudbreak-deployment/
vi Profile</pre>

2. Add the following entry, setting it to the provider that you  would like to see. For example, if you would like to see Azure only, set this to "AZURE":

    <pre>export CB_ENABLEDPLATFORMS=AZURE</pre>

    Accepted values are:
    
    * AZURE
    * AWS
    * GCP
    * OPENSTACK

    Any combination of platforms can be used; for example if you would like to see AWS and OpenStack, then use:
    
    <pre>export CB_ENABLEDPLATFORMS=AWS,OPENSTACK</pre>

    If you want to reverse the change and see all providers, then either delete CB_ENABLEDPLATFORMS from the Profile or add the following: 
    
    <pre>export CB_ENABLEDPLATFORMS=AZURE,AWS,GCP,OPENSTACK</pre>
    
3. Restart Cloudbreak by using `cbd restart`.      




## Modify an existing Cloudbreak credential

You can modify an existing Cloudbreak credential by following these steps.

> The value of the "Name" parameter cannot be changed.    
> The values of sensitive parameters will not be displayed and you will have to reenter them.    

**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Click on <img src="../images/cb_edit.png" alt="On" />  next to the credential that you want to edit.  
3. When done making changes, click **Save** to save your changes.  


## Set a default Cloudbreak credential

If using multiple Cloudbreak credentials, you can select one credential and use it as default for creating clusters. This default credential will be pre-selected in the create cluster wizard.
 
**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Click **Set as default** next to the credential that you would like to set as default.  
3. Click **Yes** to confirm. 




## Configure SMTP email notifications 

If you want to configure email notification, configure SMTP parameters in your `Profile`. 

> In order to use this configuration, your email server must use SMTP. 

The default values of the SMTP parameters are:

```
export CLOUDBREAK_SMTP_SENDER_USERNAME=
export CLOUDBREAK_SMTP_SENDER_PASSWORD=
export CLOUDBREAK_SMTP_SENDER_HOST=
export CLOUDBREAK_SMTP_SENDER_PORT=25
export CLOUDBREAK_SMTP_SENDER_FROM=
export CLOUDBREAK_SMTP_AUTH=true
export CLOUDBREAK_SMTP_STARTTLS_ENABLE=true
export CLOUDBREAK_SMTP_TYPE=smtp
```

For example: 

```
export CLOUDBREAK_SMTP_SENDER_USERNAME='myemail@gmail.com'  
export CLOUDBREAK_SMTP_SENDER_PASSWORD='Mypassword123'  
export CLOUDBREAK_SMTP_SENDER_HOST='smtp.gmail.com'  
export CLOUDBREAK_SMTP_SENDER_PORT=25  
export CLOUDBREAK_SMTP_SENDER_FROM='myemail@gmail.com'  
export CLOUDBREAK_SMTP_AUTH=true  
export CLOUDBREAK_SMTP_STARTTLS_ENABLE=true  
export CLOUDBREAK_SMTP_TYPE=smtp  
```

> The example assumes that you are using gmail. You should use the settings appropriate for your SMTP server.

If your SMTP server uses SMTPS, you must set the protocol in your `Profile` to smtps:

```
export CLOUDBREAK_SMTP_TYPE=smtps
```






## Disabling SmartSense telemetry 

Help us make a better product by opting in to automatically send information to Hortonworks. This includes enabling
Hortonworks SmartSense and sending performance and usage info. As you use the product,
SmartSense measures and collects information and then sends these information bundles to Hortonworks.

### Disable bundle upload for Cloudbreak and new clusters

> Do not perform these steps when you have clusters currently in the process of being deployed. Wait for all clusters to be deployed.


1. SSH into the Cloudbreak host.

1. Edit `/var/lib/cloudbreak-deployment/Profile`.

2. Change `CB_SMARTSENSE_CONFIGURE` to `false`:<br>
    <pre><code>export CB_SMARTSENSE_CONFIGURE=false</code></pre>

2. Restart the cloud controller:<br>
    <pre><code>cd /var/lib/cloudbreak-deployment
cbd restart</code></pre>
    

### Disable bundle upload for an existing cluster

1. SSH into the master node for the cluster.

1. Edit `/etc/hst/conf/hst-server.ini`.

2. Change `[gateway]` configuration to `false`:<br>
    <pre><code>[gateway]
enabled=false</code></pre>

2. Restart the SmartSense Server:
    <pre><code>hst restart</code></pre>
    
3. (Optional) Disable SmartSense daily bundle capture:

    * SmartSense is scheduled to capture a telemetry bundle daily. With the bundle upload disabled, the bundle will still
    be captured but just saved locally (i.e. not uploaded).
    * To disable the bundle capture, execute the following:
    <pre><code>hst capture-schedule -a pause</code></pre>

3. Repeat on all existing clusters.




## Customizing Cloudbreak Profile file  

Cloudbreak deployer configuration is based on environment variables.  

During startup, Cloudbreak deployer tries to determine the underlying infrastructure and then sets required environment variables with appropriate default values. If these environment variables are not sufficient for your use case, you can set additional environment variables in your `Profile` file. 


### Set Profile variables

To set environment variables relevant for Cloudbreak Deployer, add them to a file called `Profile` located in the Cloudbreak deployment directory (typically `/var/lib/cloudbreak-deployment`).

The `Profile` file is sourced, so you can use the usual syntax to set configuration values:

```
export MY_VAR=some_value
export MY_OTHER_VAR=another_value 
```

After changing a property, you must regenerate the config file and restart the application by using `cbd restart`.


### Check available Profile variables

To see all available environment variables with their default values, use:

```
cbd env show
```

### Create environment-specific Profile files 

If you would like to use a different versions of Cloudbreak for prod and qa profile, you must create two environment specific configurations that can be sourced. For example:

* Profile.prod  
* Profile.qa   

For example, to create and use a prod profile, you need to:

1. Create a file called `Profile.prod`  
2. Write the environment-specific `export DOCKER_TAG_CLOUDBREAK=0.3.99` into `Profile.prod` to specify Docker image.  
3. Set the environment variable: `CBD_DEFAULT_PROFILE=prod`  

To use the prod specific profile once, set:  

<pre>CBD_DEFAULT_PROFILE=prod cbd some_commands</pre>
    
To permanently use the prod profile, set `export CBD_DEFAULT_PROFILE=prod` in your `.bash_profile`.


### Secure the Profile file 

Before starting Cloudbreak for the first time, configure the Profile file as directed below. Changes are applied during startup so a restart (`cbd restart`) is required after each change.

1. Execute the following command in the directory where you want to store Cloudbreak-related files:

    <pre>
echo export PUBLIC_IP=[the ip or hostname to bind] > Profile
</pre>

[comment]: <> (TO-DO: Do you mean that this needs to be executed in the deployment directory? Or?)

2. After you have a base Profile file, add the following custom properties to it:

    <pre>
export UAA_DEFAULT_SECRET='[custom secret]'
export UAA_DEFAULT_USER_EMAIL='[default admin email address]'
export UAA_DEFAULT_USER_PW='[default admin password]'
export UAA_DEFAULT_USER_FIRSTNAME='[default admin first name]'
export UAA_DEFAULT_USER_LASTNAME='[default admin last name]'
</pre>

    Cloudbreak has additional secrets which by default inherit their values from `UAA_DEFAULT_SECRET`. Instead of using the default, you can define different values in the Profile for each of these service clients:

    <pre>
export UAA_CLOUDBREAK_SECRET='[cloudbreak secret]'
export UAA_PERISCOPE_SECRET='[auto scaling secret]'
export UAA_ULUWATU_SECRET='[web ui secret]'
export UAA_SULTANS_SECRET='[authenticator secret]'
</pre>

    You can change these secrets at any time, except `UAA_CLOUDBREAK_SECRET` which is used to encrypt sensitive information at database level. 
    
[comment]: <> (TO-DO: The info below is explained in a way that is confusing. Can you rephrase?) 

    `UAA_DEFAULT_USER_PW` is stored in plain text format, but if `UAA_DEFAULT_USER_PW` is missing from the Profile, it gets a default value. Because default password is not an option, if you set an empty password explicitly in the Profile Cloudbreak deployer will ask for password all the time when it is needed for the operation.

    <pre>
export UAA_DEFAULT_USER_PW=''
</pre>

    In this case, Cloudbreak deployer wouldn't be able to add the default user, so you have to do it manually by executing the following command:

    <pre>
cbd util add-default-user
</pre>

For more information about setting environment variables in Profile, refer to [Set Profile variables](#set-profile-variables).


### Configuring Consul 

Cloudbreak uses [Consul](https://www.consul.io/) for DNS resolution. All Cloudbreak related services are registered as someservice.service.consul.

Consul’s built-in DNS server is able to fallback on another DNS server. This option is called `-recursor`. Cloudbreak deployer first tries to discover the DNS settings of the host by looking for nameserver entry in the `/etc/resolv.conf` file. If it finds one, consul will use it as a recursor. Otherwise, it will use `8.8.8.8`.

For a full list of available consul config options, refer to [Consul documentation](https://www.consul.io/docs/agent/options.html).

To pass any additional Consul configuration, define the `DOCKER_CONSUL_OPTIONS` variable in the Profile file.


## Manually import HDP and HDF images to OpenStack

An OpenStack administrator must perform these steps to add the Cloudbreak deployer image to your OpenStack deployment. 

> Importing prewarmed and base HDP and HDF images is no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create a cluster.

### Import HDP prewarmed image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDP cluster. If you would like to import HDP prewarmed images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDP image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1805181129.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp-26-1805181129.img
export CB_LATEST_IMAGE_NAME=cb-hdp-26-1805181129.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images.


### Import HDF prewarmed image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDF cluster. If you would like to import HDP prewarmed images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDF image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-31-1805251001.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp-31-1805251001.img
export CB_LATEST_IMAGE_NAME=cb-hdp-31-1805251001.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images. 


### Import HDP/HDF base image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDF cluster. If you would like to import HDP/HDF base images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDP image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp--1801261636.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp--1801261636.img
export CB_LATEST_IMAGE_NAME=cb-hdp--1801261636.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images.





