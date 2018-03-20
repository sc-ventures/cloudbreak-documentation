## Configuring External Database  

Cloudbreak allows you to register an existing RDBMS instance to be used for a database for the following services:   

* Hive  
* Druid  
* Ranger  
* Superset  
* Oozie  
* Hue 

You must create the external RDBMS instance and database prior to registering it with Cloudbreak. Once you have it ready, you can:

1. Register it in Cloudbreak web UI or CLI.  
2. Use it with one or more clusters. Once registered, the database will now show up in the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**.  

> Only **PostgreSQL** is supported at this time. 


### Register an External Database 

Follow these steps to register an external RDBMS instance with Cloudbreak. 

> You must create the external RDBMS instance and database prior to registering it with Cloudbreak.

**Steps** 

1. From the navigation pane, select **External Services** > **Database Configurations**.  
2. Select **Register Database Configuration**.    
5. Provide the following information:

    | Parameter | Description |
    |:---|:---|
    | Name | Enter the name to use when registering this database to Cloudbreak. This is **not** the database name. |
    | Type | Select the service for which you would like to use the external database. |
    | JDBC Connection | Select the database **type** (PostgreSQL) and enter the **JDBC connection** string (HOST:PORT/DB_NAME).  |
    | Username | Enter the JDBC connection username and password. |
    | Password | Enter the JDBC connection password. |

6. Click **Test Connection** to validate and test the RDS connection information.  
7. Once your settings are validated and working, click **REGISTER** to save the configuration. The database will now show up in the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**.  


