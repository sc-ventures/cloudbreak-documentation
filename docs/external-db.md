## Using an external database for cluster services  

Cloudbreak allows you to register an existing RDBMS instance as an [external source](concepts.md#external-sources) to be used for a database for certain services. After you register the RDBMS with Cloudbreak, you can use it for multiple clusters. 

### Supported databases
 
If you would like to use an external database for one of the components that support it, you may use the database types and versions defined in the [Support Matrix](https://supportmatrix.hortonworks.com/).  

| Component | Embedded DB available? | How to set up your own DB |
|---|---|---|
| **Ambari**   | By default, Ambari uses an embedded PostgreSQL instance.  | Refer to [​Using Existing Databases - Ambari](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.2.2/bk_ambari-administration/content/using_existing_databases_-_ambari.html) or to documentation for the specific version that you would like to use. |
| **Druid**    | You must provide an external database. | Refer to [Configuring Druid and Superset Metadata Stores in MySQL](https://docs.hortonworks.com/HDPDocuments/HDF3/HDF-3.1.2/bk_installing-hdf-ppc/content/configure-druid-database-mysql.html) or to documentation for the specific version that you would like to use. |
| **Hive**     | By default, Cloudbreak installs a PostgreSQL instance on the Hive Metastore host. | Refer to [​Using Existing Databases - Hive](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.2.2/bk_ambari-administration/content/using_new_and_existing_databases_-_hive.html) or to documentation for the specific version that you would like to use. |
| **Oozie**    | You must provide an external database. | Refer to [​Using Existing Databases - Oozie](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.2.2/bk_ambari-administration/content/using_existing_databases_-_oozie.html) or to documentation for the specific version that you would like to use. |
| **Ranger**   | You must provide an external database. | Refer to [​Using Existing Databases - Ranger](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_security/content/configuring_database_instance.html) or to documentation for the specific version that you would like to use. |
| **Superset** | You must provide an external database. | Refer to [Configuring Druid and Superset Metadata Stores in MySQL](https://docs.hortonworks.com/HDPDocuments/HDF3/HDF-3.1.2/bk_installing-hdf-ppc/content/configure-druid-database-mysql.html) or to documentation for the specific version that you would like to use. |
| **Other**    | Refer to the component-specific documentation. | Refer to the component-specific documentation. |

[Comment]: <> (During the demo, Richard mentioned that in addition to these, Ambari supports Oracle; however, in CLoudbreak we have a problem automating one of the steps (related to Oracle tools). This issue is pending, but as of April 16 we cannot support Oracle for Ambari.)

[Comment]: <> (Also Attila mentioned that there were some problems with Ranger. Did anything change here that may need to be documented?)

### External database options  

Cloudbreak includes the following external database options:

| Option | Description | Blueprint requirements | Steps | Example |
|---|---|---|---|---|
| **Built-in types** | Cloudbreak includes a few built-in types: Hive, Druid, Ranger, Superset, and Oozie. | Use a standard blueprint which does not include any JDBC  parameters. Cloudbreak automatically injects the JDBC property variables into the blueprint. | Simply [register the database in the UI](#register-an-external-database). After that, you can attach the database config to your clusters. | Refer to [Example 1](#example-1-built-in-type-hive) |
| **Other types** | In addition to the built-in types, Cloudbreak allows you to specify custom types. In the UI, this corresponds to the UI option is called "Other" > "Enter the type". | You must provide a custom dynamic blueprint which includes RDBMS-specific variables. Refer to [Creating a template blueprint](blueprints.md#creating-a-template-blueprint-for-rdmbs). | Prepare your custom blueprint first. Next, [register the database in the UI](#register-an-external-database). After that, you can attach the database config to your clusters. | Refer to [Example 2](#example-2-other-type) | 

During cluster create, Cloudbreak checks whether the JDBC properties are present in the blueprint:

<a href="../images/cb_cb-rdbms-diagram.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-rdbms-diagram.png" width="550" title="Cloudbreak web UI"></a>

[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)  

  
#### Example 1: Built-in type Hive 

In this scenario, you start up with a standard blueprint, and Cloudbreak injects the JDBC properties into the blueprint.

1. Register an existing external database of "Hive" type (built-in type):

    <a href="../images/cb_cb-rdbms-e1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-rdbms-e1.png" width="550" title="Cloudbreak web UI"></a>
    
[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)
    
    | Property variable | Example value |
|---|---|
| rds.hive.connectionString | jdbc:postgresql://ec2-54-159-202-231.compute-1.amazonaws.com:5432/hive |
| rds.hive.connectionDriver | org.postgresql.Driver | 
| rds.hive.connectionUserName | myuser |
| rds.hive.connectionPassword | Hadoop123! |
| rds.hive.fancyName | PostgreSQL |
| rds.hive.databaseType | postgres |

[Comment]: <> (Is the rds.hive.connectionDriver parameter deprecated?) 

2. Create a cluster by using a standard blueprint (i.e. one without JDBC related variables) and by attaching the external Hive database configuration.  

3. Upon cluster create, Hive JDBC properties will be injected into the blueprint according to the following template:

    <pre>...
"hive-site": {
    "properties": {
      "javax.jdo.option.ConnectionURL": "{{{ rds.hive.connectionString }}}",
      "javax.jdo.option.ConnectionDriverName": "{{{ rds.hive.connectionDriver }}}",
      "javax.jdo.option.ConnectionUserName": "{{{ rds.hive.connectionUserName }}}",
      "javax.jdo.option.ConnectionPassword": "{{{ rds.hive.connectionPassword }}}"
    }
  },
  "hive-env" : {
    "properties" : {
      "hive_database" : "Existing {{{ rds.hive.fancyName}}} Database",
      "hive_database_type" : "{{{ rds.hive.databaseType }}}"
    }
}
...</pre> 

[Comment]: <> (Is the rds.hive.connectionDriver parameter deprecated?) 


#### Example 2: Other type 

In this scenario, you start up with a special blueprint including JDBC property variables, and Cloudbreak replaces JDBC-related property variables in the blueprint. 

1. Prepare a blueprint blueprint that includes property variables. Use [mustache template](https://mustache.github.io/) syntax. For example:

    <pre>...
"test-site": {
      "properties": {
       "javax.jdo.option.ConnectionURL":"{{rds.test.connectionString}}"
      }
...</pre>


1. Register an existing external database of some "Other" type. For example:

    <a href="../images/cb_cb-rdbms-e2.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-rdbms-e2.png" width="550" title="Cloudbreak web UI"></a>
    
[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)
    
    | Property variable | Example value |
|---|---|
| rds.hive.connectionString| ec2-54-159-202-231.compute-1.amazonaws.com:5432/hive |
| rds.hive.connectionDriver | org.postgresql.Driver | 
| rds.hive.connectionUserName | myuser |
| rds.hive.connectionPassword | Hadoop123! |
| rds.hive.subprotocol | postgres |
| rds.hive.databaseEngine | POSTGRES |


2. Create a cluster by using your custom blueprint and by attaching the external database configuration.  

3. Upon cluster create, Cloudbreak replaces JDBC-related property variables in the blueprint. 

**Related links**  
[Mustache template syntax](https://mustache.github.io/)  

[Comment]: <> (Are these examples still correct or did some of the parameters change in this release?) 
  
  
### Creating a template blueprint for RDMBS  

In order to use an external RDBMS for some component other than the built-in components, you must include JDBC property variables in your blueprint. You must use [mustache template](https://mustache.github.io/) syntax. See [Example 2: Other type](#example-2-other-type) for an example configuration. 

**Related links**  
[Creating a template blueprint](blueprints.md#creating-a-template-blueprint)  
[Mustache template syntax](https://mustache.github.io/) (External)  

[Comment]: <> (Does this blueprint doc have to be updated to include "Connector's JAR URL" parameter when using MySQL or Oracle?)

[Comment]: <> (Based on the CLI, OI suspect that "rds.[type].connectionURL" may be deprecated now?)


### Register an external database    
  
You must create the external RDBMS instance and database prior to registering it with Cloudbreak. Once you have it ready, you can:

1. Register it in Cloudbreak web UI or CLI.  
2. Use it with one or more clusters. Once registered, the database will now show up in the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**.  

**Prerequisites**  

If you are planning to use an external MySQL or Oracle database, you must download the JDBC connector's JAR file and place it in a location available to the cluster host on which Ambari is installed. The steps below require that you provide the URL to the JDBC connector's JAR file. 

> If you are using your own [custom image](images.md), you may place the JDBC connector's JAR file directly on the machine as part of the image burning process. 

**Steps** 

1. From the navigation pane, select **External Sources** > **Database Configurations**.  
2. Select **Register Database Configuration**.    
3. Provide the following information:

    | Parameter | Description |
    |:---|:---|
    | Name | Enter the name to use when registering this database to Cloudbreak. This is **not** the database name. |
    | Type | Select the service for which you would like to use the external database. If you selected "Other", you must provide a special blueprint. |
    | JDBC Connection | Select the database **type** and enter the **JDBC connection** string (HOST:PORT/DB_NAME).  |
    | Connector's JAR URL | (MySQL and Oracle only) Provide a URL to the JDBC connector's JAR file. The JAR file must be hosted in a location accessible to the cluster host on which Ambari is installed. At cluster creation time, Cloudbreak places the JAR file in the /opts/jdbc-drivers directory. You do not need to provide the "Connector's JAR URL if you are using a custom image and the JAR file was either manually placed on the VM as part of custom image burning or it was placed there by using a recipe. |
    | Username | Enter the JDBC connection username. |
    | Password | Enter the JDBC connection password. |

4. Click **Test Connection** to validate and test the RDS connection information.  

    <div class="note">
    <p class="first admonition-title">Note</p>
    <p class="last">The <b>Test Connection</b> option:<ul><li>Does not work when an external authentication source uses LDAPS with a self-signed certificate.</li><li>Might not work if Cloudbreak instance cannot reach the LDAP server instance.</li></ul>In these cases, ignore the error and proceed with cluster installation.</p>
</div> 

5. Once your settings are validated and working, click **REGISTER** to save the configuration.  
6. The database will now show up on the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**. You can select it and click **Attach** each time you would like to use it for a cluster.  



