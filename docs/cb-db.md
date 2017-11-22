
## Manage Cloudbreak Database

By default, Cloudbreak uses a built-in PostgreSQL database to persist data. For production environments, we suggest that you use an external database, an RDS served by your cloud provider. However, if you choose to use the default database, you should know that Cloudbreak deployer includes features for dumping and restoring built-in databases.


### Dump and Restore Cloudbreak Database 

Cloudbreak deployer uses Docker for the underlying infrastructure and uses Docker volume for storing data. There are two separate volumes: 

* a volume called `common` for storing live data  
* a volume called `cbreak_dump` for database dumps 

You can override default live data volume any time by extending your `Profile` with the following variable:

<pre><small>export COMMON_DB_VOL="my-live-data-volume"
</small></pre>

To create database dumps, execute the following commands:

<pre><small>cbd db dump common cbdb
cbd db dump common uaadb
cbd db dump common periscopedb
</small></pre>

The dump command has an optional third parameter, the `name` of the dump. If you give your dump a name, Cloudbreak deployer will create a symbolic link which points to the SQL dump. For example: 

<pre><small>cbd db dump common cbdb name-of-the-dump
</small></pre>

To list existing dumps, execute the `cbd db list-dumps` command. Each kind of database dump (cbdb, uaadb, periscopedb) has a link to the latest dump on the `cbreak_dump` volume. During the restore process, Cloudbreak deployer restores from latest dump. To check which dump is the latest, execute:

<pre><small>docker run --rm -v cbreak_dump:/dump -it alpine ls -lsa /dump/cbdb/latest
</small></pre>

To set any of the existing dumps as latest use the `set-dump` command. You can set both regular or named dumps. For example: 

<pre><small>cbd db set-dump cbdb 20170628_1805
</small></pre>
or
<pre><small>cbd db set-dump cbdb name-of-the-dump
</small></pre>

To remove the existing `common` volume, stop all the related Cloudbreak containers with `cbd kill` command, and then remove the volume:

<pre><small>docker volume rm common
</small></pre>

To restore databases from dumps, execute:

<pre><small>cbd db restore-volume-from-dump common cbdb
cbd db restore-volume-from-dump common uaadb
cbd db restore-volume-from-dump common periscopedb
</small></pre>

To save your dumps to the host machine, execute:

<pre><small>docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/cbdb/latest/dump.sql > cbdb.sql
docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/uaadb/latest/dump.sql > uaadb.sql
docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/periscopedb/latest/dump.sql > periscopedb.sql
</small></pre>


### Using an External Database for Cloudbreak 

By default Cloudbreak uses a built-in PostgreSQL database but you can optionally configure Cloudbreak to use an external database, typically one provided by a cloud provider.

#### External Database Support Matrix

| Type | Version supported |
|---|---| 
| Default (embedded) | 9.6.1 |
| External PostgreSQL | 9.x |
| External MySQL | Not supported |
| External MariaDB | Not supported |
| External Oracle | Not supported |
| External SQL Server | Not supported |

#### Configure External Database for Cloudbreak 

To configure an external PostgreSQL database for Cloudbreak, perform these steps. 

**Steps**

1. Stop your running Cloudbreak application by using the `cbd kill` command.   
2. Create three database dumps (cbdb, uaadb, periscopedb) and save them to the host:  

    <pre><small>cbd db dump common cbdb
cbd db dump common uaadb
cbd db dump common periscopedb</small></pre>

    <pre><small>docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/cbdb/latest/dump.sql > cbdb.sql
docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/uaadb/latest/dump.sql > uaadb.sql
docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/periscopedb/latest/dump.sql > periscopedb.sql</small></pre>


3. Log in to the management interface of your external database.
 
4. On your external database, create three databases: `cbdb, uaadb, periscopedb`. If you would not like to change any database specifics (such as Owner, Tablespace), you can create these databases using the following commands:
   
    <pre>create database cbdb;
create database uaadb;
create database periscopedb;</pre>
        
    For more information refer to the [PostgreSQL documentation](https://www.postgresql.org/docs/9.6/static/sql-createdatabase.html).

5. Exit the external database's management interface.  
    
6. Import the previously exported dump for each database. This can be done by executing the following commands:

    <pre><small>docker exec -i cbreak_commondb_1 psql -U postgres cbdb < cbdb.sql
    docker exec -i cbreak_commondb_1 psql -U postgres uaadb < uaadb.sql
    docker exec -i cbreak_commondb_1 psql -U postgres periscopedb < periscopedb.sql</small></pre>
    
    For more information refer to the [PostgreSQL documentation](https://www.postgresql.org/docs/9.6/static/backup-dump.html#BACKUP-DUMP-RESTORE).
    
7. Set the following variables in Cloudbreak Profile file.  
    Modify the RDS_URL parameter by passing the ENDPOINT:PORT of your external database. Do not need to modify values of any other parameters, as their values are derived from the RDS_URL.

    <pre><small>export RDS_URL=localhost:5432
    
    export CB_DB_PORT_5432_TCP_ADDR=${RDS_URL%%:*}
    export CB_DB_PORT_5432_TCP_PORT=${RDS_URL##*:}
    export CB_DB_ENV_USER=postgres
    export CB_DB_ENV_PASS=postgres
    export CB_DB_ENV_DB=cloudbreak_database_name
    
    export PERISCOPE_DB_TCP_ADDR=${RDS_URL%%:*}
    export PERISCOPE_DB_TCP_PORT=${RDS_URL##*:}
    export PERISCOPE_DB_USER=postgres
    export PERISCOPE_DB_PASS=postgres
    export PERISCOPE_DB_NAME=autoscale_database_name
    export PERISCOPE_DB_SCHEMA_NAME=autoscale_schema_name
    
    export IDENTITY_DB_URL=$RDS_URL
    export IDENTITY_DB_USER=postgres
    export IDENTITY_DB_PASS=postgres
    export IDENTITY_DB_NAME=ideontity_database_name</small></pre>

8. Start Cloudbreak application by using the `cbd start` command. 

[Comment]: <> (@Peter D, How can I verify these steps worked?)

