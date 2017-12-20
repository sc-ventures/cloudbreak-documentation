## Configuring External Cloudbreak Database

By default, Cloudbreak uses an embedded PostgreSQL database to persist data related to Cloudbreak
configuration, setup and so on. For a production Cloudbreak deployment, we suggest that you configure an external database. 

**External Database Support Matrix**

An embedded PostgreSQL 9.6.1 database is used by Cloudbreak by default. If you would like to
use an external database for Cloudbreak, you may use the following supported database types and versions: 

| Database Type | Supported Version |
|---|---| 
| External PostgreSQL | 9.x |
| External MySQL | Not supported |
| External MariaDB | Not supported |
| External Oracle | Not supported |
| External SQL Server | Not supported |

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

<div class="note">		
<p class="first admonition-title">Data Migration</p>  
<p class="last"> If you want to migrate your existing data (such as  blueprints, recipes, and so on) from the embedded database to the external one, then after completing these steps, you should also create a <a href="../cb-migrate/#back-up-cloudbreak-database">backup</a> of your original database and then <a href="../cb-migrate#populate-database-with-dump-from-original-cloudbreak-instance">restore</a> it in the external database.   		
</p>		
</div>


