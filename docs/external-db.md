## Using an external database for cluster services  

Cloudbreak allows you to register an existing RDBMS instance to be used for a database for certain services. After you register the RDBMS with Cloudbreak, you can use it for multiple clusters. 

> Only **PostgreSQL** is supported at this time.  

Cloudbreak includes the following external database options:

| Option | Description | Blueprint Requirements | Steps | Example |
|---|---|---|---|---|
| **Built-in types** | Cloudbreak includes a few built-in types: Hive, Druid, Ranger, Superset, and Oozie. | Use a standard blueprint which does not include any JDBC  parameters. Cloudbreak automatically injects the JDBC property variables into the blueprint. | Simply [register the database in the UI](#register-an-external-database). After that, you can attach the database config to your clusters. | Refer to [Example 1](#example-1-built-in-type-hive) |
| **Other types** | In addition to the built-in types, Cloudbreak allows you to specify custom types. In the UI, this corresponds to the UI option is called "Other" > "Enter the type". | You must provide a custom dynamic blueprint which includes RDBMS-specific variables. Refer to [Creating a template blueprint](blueprints.md#creating-a-template-blueprint-for-rdmbs). | Prepare your custom blueprint first. Next, [register the database in the UI](#register-an-external-database). After that, you can attach the database config to your clusters. | Refer to [Example 2](#example-2-other-type) | 

During cluster create, Cloudbreak checks whether the JDBC properties are present in the blueprint:

<a href="../images/cb_cb-rdbms-diagram.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-rdbms-diagram.png" width="550" title="Cloudbreak web UI"></a>

  
### Example 1: Built-in type Hive 

In this scenario, you start up with a standard blueprint, and Cloudbreak injects the JDBC properties into the blueprint.

1. Register an existing external database of "Hive" type (built-in type):

    <a href="../images/cb_cb-rdbms-e1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-rdbms-e1.png" width="550" title="Cloudbreak web UI"></a>
    
    | Property Variable | Example Value |
|---|---|
| rds.hive.connectionURL | jdbc:postgresql://hive.test.eu-west-1:5432/hive |
| rds.hive.connectionDriver | org.postgresql.Driver | 
| rds.hive.connectionUserName | mydatabaseuser |
| rds.hive.connectionPassword | Hadoop123! |
| rds.hive.subprotocol | postgres |
| rds.hive.databaseEngine | POSTGRES |

2. Create a cluster by using a standard blueprint (i.e. one without JDBC related variables) and by attaching the external Hive database configuration.  

3. Upon cluster create, Hive JDBC properties will be injected into the blueprint according to the following template:

    <pre>...
"hive-site": {
     "properties": {
       "javax.jdo.option.ConnectionURL": "{{{ rds.hive.connectionURL }}}",
       "javax.jdo.option.ConnectionDriverName": "{{{ rds.hive.connectionDriver }}}",
       "javax.jdo.option.ConnectionUserName": "{{{ rds.hive.connectionUserName }}}",
       "javax.jdo.option.ConnectionPassword": "{{{ rds.hive.connectionPassword }}}"
      }
    },
"hive-env" : {
     "properties" : {
       "hive_database" : "Existing {{{ rds.hive.subprotocol }}} Database",
       "hive_database_type" : "{{{ rds.hive.databaseEngine }}}"
      }
}
...</pre> 



### Example 2: Other type 

In this scenario, you start up with a special blueprint including JDBC property variables, and Cloudbreak replaces JDBC-related property variables in the blueprint. 

1. Prepare a blueprint blueprint that includes property variables. Use [mustache template](https://mustache.github.io/) syntax. For example:

    <pre>...
"test-site": {
    "properties": {
       "javax.jdo.option.ConnectionURL":"{{{rds.test.connectionURL}}}"
      }
...</pre>


1. Register an existing external database of some "Other" type. For example:

    <a href="../images/cb_cb-rdbms-e2.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-rdbms-e2.png" width="550" title="Cloudbreak web UI"></a>
    
    | Property Variable | Example Value |
|---|---|
| rds.hive.connectionURL | db.test.eu-west-1:5432/sometest |
| rds.hive.connectionDriver | org.postgresql.Driver | 
| rds.hive.connectionUserName | mydatabaseuser |
| rds.hive.connectionPassword | Hadoop123! |
| rds.hive.subprotocol | postgres |
| rds.hive.databaseEngine | POSTGRES |

2. Create a cluster by using your custom blueprint and by attaching the external database configuration.  

3. Upon cluster create, Cloudbreak replaces JDBC-related property variables in the blueprint. 

**Related links**  
[Mustache template syntax](https://mustache.github.io/)  
  
  
### Creating a template blueprint for RDMBS  

In order to use an external RDBMS for some component other than the built-in components, you must include JDBC property variables in your blueprint. You must use [mustache template](https://mustache.github.io/) syntax. See [Example 2: Other type](#example-2-other-type) for an example configuration. 

**Related links**  
[Creating a template blueprint](blueprints.md#creating-a-template-blueprint)  
[Mustache template syntax](https://mustache.github.io/) (External)  


#### RDBMS Property Variables

Cloudbreak utilizes the following RDBMS related variables, where [type] can be one of [hive,druid,oozie,ranger,superset] or other specified by you (for example "beacon").

<pre>rds.[type].connectionURL
rds.[type].connectionDriver
rds.[type].connectionUserName
rds.[type].connectionPassword
rds.[type].databaseName
rds.[type].host
rds.[type].hostWithPort
rds.[type].subprotocol
rds.[type].databaseEngine</pre>

These parameters correspond to the external database UI and CLI options. 


### Register an external database    
  
You must create the external RDBMS instance and database prior to registering it with Cloudbreak. Once you have it ready, you can:

1. Register it in Cloudbreak web UI or CLI.  
2. Use it with one or more clusters. Once registered, the database will now show up in the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**.  


**Steps** 

1. From the navigation pane, select **External Sources** > **Database Configurations**.  
2. Select **Register Database Configuration**.    
3. Provide the following information:

    | Parameter | Description |
    |:---|:---|
    | Name | Enter the name to use when registering this database to Cloudbreak. This is **not** the database name. |
    | Type | Select the service for which you would like to use the external database. If you selected "Other", you must provide a special blueprint. |
    | JDBC Connection | Select the database **type** (PostgreSQL) and enter the **JDBC connection** string (HOST:PORT/DB_NAME).  |
    | Username | Enter the JDBC connection username. |
    | Password | Enter the JDBC connection password. |

4. Click **Test Connection** to validate and test the RDS connection information.  
5. Once your settings are validated and working, click **REGISTER** to save the configuration.  
6. The database will now show up on the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**. You can select it each time you would like to use it for a cluster.  



