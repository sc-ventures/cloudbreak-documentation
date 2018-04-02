## Blueprints

**Ambari blueprints** are your declarative definition of your HDP or HDF cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. 

You have two options concerning using blueprints with Cloudbreak:

* **Use one of the pre-defined blueprints**: To use one of the default blueprints, simply select them when creating a cluster. The option is available on the **General Configuration** page. First select the **Stack Version** and then select your chosen blueprint under **Cluster Type**. For the list of default blueprints, refer to [Default Cluster Configurations](index.md#default-cluster-configurations).       
* **Add your custom blueprint** by uploading a JSON file or pasting the JSON text. 

We recommend that you review the default blueprints to check if they meet your requirements. You can do this by selecting **Blueprints** from the navigation pane in the Cloudbreak web UI or by reading the documentation below.
 

### Using Custom Blueprints 

This option allows you to create and save your custom blueprints. 

#### Creating a Blueprint

Ambari blueprints are specified in the JSON format. A blueprint can be exported from a running Ambari cluster and can be reused in Cloudbreak after slight modifications. When a blueprint is exported, it includes  some hardcoded configurations such as domain names, memory configurations, and so on, that are not applicable to the Cloudbreak cluster. There is no automatic way to modify an exported blueprint and make it instantly usable in Cloudbreak, the modifications have to be done manually. 

In general, the blueprint should include the following elements:

<pre>"Blueprints": {
    "blueprint_name": "hdp-small-default",
    "stack_name": "HDP",
    "stack_version": "2.6"
  },
  "settings": [],
  "configurations": [],
  "host_groups": [
  {
      "name": "master",
      "configurations": [],
      "components": []
    },
    {
      "name": "worker",
      "configurations": [],
      "components": [ ]
    },
    {
      "name": "compute",
      "configurations": [],
      "components": []
    }
   ]
  }</pre>
  
For correct blueprint layout and other information about Ambari blueprints, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page. 

<div class="note">
    <p class="first admonition-title">Creating Blueprints for Ambari 2.6.1+</p>
    <p class="last">
Ambari 2.6.1 or newer does not install the mysqlconnector; therefore, when creating a blueprint for Ambari 2.6.1 or newer <b>you should not include the MYSQL_SERVER component</b> for Hive Metastore in your blueprint. Instead, you have two options:
<ul>
<li>Configure an external RDBMS instance for Hive Metastore and include the JDBC connection information in your blueprint. If you choose to use an external database that is not PostgreSQL (such as Oracle, mysql) you must also set up Ambari with the appropriate connector; to do this, create a pre-ambari-start recipe and pass it when creating a cluster.</li>
<li>If a remote Hive RDBMS is not provided, Cloudbreak installs a Postgres instance and configures it for Hive Metastore during the cluster launch.</li>
</ul>
For information on how to configure an external database and pass your external database connection parameters, refer to <a href="https://cwiki.apache.org/confluence/display/AMBARI/Blueprints">Ambari blueprint</a> documentation.
</p>
</div>

Cloudbreak requires you to define an additional element in the blueprint called "blueprint_name". This should be a unique name within Cloudbreak list of blueprints. For example:

<pre>"Blueprints": {
    "blueprint_name": "hdp-small-default",
    "stack_name": "HDP",
    "stack_version": "2.6"
  },
  "settings": [],
  "configurations": [],
  "host_groups": [
  ...
</pre>

The "blueprint_name" is not included in the Ambari export. 

After you provide the blueprint to Cloudbreak, the host groups in the JSON will be mapped to a set of instances when starting the cluster, and the specified services and components will be installed on the corresponding nodes. It is not necessary to define a complete configuration in the blueprint. If a configuration is missing, Ambari will use a default value. 

Here are a few [blueprint examples](https://github.com/hortonworks/cloudbreak/tree/master/core/src/main/resources/defaults/blueprints). You can also refer to the default blueprints provided in the Cloudbreak UI.

**Related Links**  
[Blueprint Examples](https://github.com/hortonworks/cloudbreak/tree/master/core/src/main/resources/defaults/blueprints) (Hortonworks)     
[Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) (External)  
 

[comment]: <> (TO-DO: Maybe we can find some newer examples?)


#### Creating a Dynamic Blueprint Template   

[Comment]: <> (Draft from Richard. Need to edit.)

Cloudbreak allows you to create special blueprints which include templating: the values of the variables specified in the blueprint will be dynamically generated in the cluster creation phase, picking up the parameter values that you provided in the UI or CLI. Cloudbreak supports mustache kind of templating with the {{{variable}}} syntax.
You cannot use functions in the blueprint file; only variable injection is supported.

The following variables can be specified:

- LDAP properties (when assigning an external LDAP to a cluster):
    - ldap.connectionURL - the URL of the LDAP (host:port)  
    - ldap.bindDn  
    - ldap.bindPassword  
    - ldap.directoryType  
    - ldap.userSearchBase  
    - ldap.userNameAttribute  
    - ldap.userObjectClass  
    - ldap.groupSearchBase  
    - ldap.groupNameAttribute  
    - ldap.groupObjectClass  
    - ldap.groupMemberAttribute  
    - ldap.domain  
    
- External database properties (when assigning an external database to a cluster):
    - rds.[type].connectionURL - The jdbc url to the RDBMS    
    - rds.[type].connectionDriver - In case of Postgres this is org.postgresql.Driver  
    - rds.[type].connectionUserName - The user name to the database     
    - rds.[type].connectionPassword - The password for the connection  
    - rds.[type].databaseName - The name of the database  
    - rds.[type].host - The host of the database  
    - rds.[type].hostWithPort - Database host + port like 10.1.1.1:1234  
    - rds.[type].subprotocol - Parsed from jdbc url like postgres  
    - rds.[type].databaseEngine - Capital database name like POSTGRES  
    
- Properties related to the gateway used for Knox:
    - gateway.enableGateway  
    - gateway.gatewayType  
    - gateway.path  
    - gateway.topologyName  
    - gateway.exposedServices  
    - gateway.ssoType  
    - gateway.ssoProvider   
    - gateway.signKey  
    - gateway.signPub  
    - gateway.signCert  
    - gateway.tokenCert  
    
- Name of the cluster:
    - cluster_name  
    
- First name of the cluster user:
    - cluster_admin_firstname  
    
- Password for Ambari:
    - cluster_admin_password  
    
- Last name of the cluster user:
    - cluster_admin_lastname  
    
- Email of the cluster user:
    - admin_email    
    
- Version of HDP or HDF:
    - stack_type_version  
    
- Type of stack (HDF or HDP):
    - stack_type  


#### Upload a Blueprint 

Once you have your blueprint ready, perform these steps.

**Steps**

1. In the Cloudbreak UI, select **Blueprints** from the navigation pane. 
2. To add your own blueprint, click **Create Blueprint** and enter the following parameters:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your blueprint. |
| Description | (Optional) Enter a description for your blueprint.|
| Blueprint Source | <p>Select one of: <ul><li>**Text**: Paste blueprint in JSON format.</li><li> **File**: Upload a file that contains the blueprint.</li><li> **URL**: Specify the URL for your blueprint.</li></ul> |

    <a href="../images/cb-blueprint-add.png" target="_blank" title="click to enlarge"><img src="../images/cb-blueprint-add.png" width="650" title="Cloudbreak web UI"></a> 

2. To use the uploaded blueprints, select it when creating a cluster. The option is available on the **General Configuration** page. First select the **Platform Version** and then select your chosen blueprint under **Cluster Type**. 

    <a href="../images/cb-blueprint-select.png" target="_blank" title="click to enlarge"><img src="../images/cb-blueprint-select.png" width="650" title="Cloudbreak web UI"></a> 



#### View Blueprint Details 

Once a blueprint has been registered in Cloudbreak, you can access its details in the Cloudbreak UI.

**Steps**

1. In the Cloudbreak UI, select **Blueprints** from the navigation pane and click on an entry to navigate to details. 

    You can view blueprint details using the **List View** and **Raw View**:

    <a href="../images/cb-blueprint-details.png" target="_blank" title="click to enlarge"><img src="../images/cb-blueprint-details.png" width="650" title="Cloudbreak web UI"></a> 


 
#### Delete Blueprint

To delete a default or custom blueprint, perform these steps.

**Steps**

1. In the Cloudbreak UI, select **Blueprints** from the navigation pane. 

2. Click on an entry to navigate to details. 

3. Click **Delete**. 

3. Confirm delete. 

