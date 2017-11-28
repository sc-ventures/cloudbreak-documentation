## Manage Cloudbreak Database

By default, Cloudbreak uses a built-in PostgreSQL database to persist data. For production environments, we suggest that you use an external database, an RDS served by your cloud provider. However, if you choose to use the default database, you should know that Cloudbreak deployer includes features for dumping and restoring built-in databases.


### Back up Cloudbreak Database 

To create a backup of the internal PostgreSQL database of Cloudbreak, perform these steps:

**Steps**

1. SSH to Cloudbreak Server instance

2. If it is not running, start the database container with `docker start cbreak_commondb_1` command

3. Execute `docker exec -it cbreak_commondb_1 bash` command to enter the container of the database.
 
4. Create three database dumps (cbdb, uaadb, periscopedb):  

    <pre><small>pg_dump -Fc -U postgres cbdb > cbdb.dump
    pg_dump -Fc -U postgres uaadb > uaadb.dump
    pg_dump -Fc -U postgres periscopedb > periscopedb.dump</small></pre>
                
5. Quit from the container with shortcut `CTRL+d`

6. Save the previously created dumps to the host instance:               

    <pre><small>docker cp cbreak_commondb_1:/cbdb.dump ./cbdb.dump
    docker cp cbreak_commondb_1:/uaadb.dump ./uaadb.dump
    docker cp cbreak_commondb_1:/periscopedb.dump ./periscopedb.dump</small></pre>


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


### Use an External Database for Cloudbreak 

By default Cloudbreak uses a built-in PostgreSQL database but you can optionally configure Cloudbreak to use an external database, typically one provided by a cloud provider.

#### External Database Support Matrix

An embedded PostgreSQL 9.6.1 database is used by Cloudbreak by default. If you would like to use an external database for Cloudbreak, you may use the following supported database types and versions: 

| Database Type | Supported Version |
|---|---| 
| External PostgreSQL | 9.x |
| External MySQL | Not supported |
| External MariaDB | Not supported |
| External Oracle | Not supported |
| External SQL Server | Not supported |


#### Configure an External Database for Cloudbreak 

To configure an external PostgreSQL database for Cloudbreak, perform these steps. 

<div class="note">
    <p class="first admonition-title">No data migration</p>
    <p class="last">
    If you do not want to migrate data from the embedded database to the external one, you should only complete steps <i>#6</i>, <i>#7</i> and <i>#9</i> and the application will automatically create the default content for the databases on the first launch. 
    </p>
</div>
  

**Steps**

1. [Create a backup](#back-up-cloudbreak-database) of the Cloudbreak database 

2. Set the following environment variables according to the settings of your external database:

    <pre><small>export RDS_URL=localhost:5432
    export RDS_USER=admin
    export PGPASSWORD=admin123
    </small></pre>
 
3. On your external database, create three databases: `cbdb, uaadb, periscopedb`. If you would not like to change any database specifics (such as Owner, Tablespace), you can create these databases using the `createdb` utility with the following commands:
   
    <pre><small>createdb -h ${RDS_URL%%:*} -p ${RDS_URL##*:} -U ${RDS_USER} cbdb
    createdb -h ${RDS_URL%%:*} -p ${RDS_URL##*:} -U ${RDS_USER} uaadb
    createdb -h ${RDS_URL%%:*} -p ${RDS_URL##*:} -U ${RDS_USER} periscopedb</small></pre>
        
    For more information refer to the [PostgreSQL documentation](https://www.postgresql.org/docs/9.6/static/app-createdb.html) . Alternatively, you can log in to the management interface of your external database and execute `create database` commands [directly](https://www.postgresql.org/docs/9.6/static/sql-createdatabase.html). 
     
4. For each database that you just created, import the previously exported dump. This can be done by executing the following commands:

    <pre><small>pg_restore -h ${RDS_URL%%:*} -p ${RDS_URL##*:} -U ${RDS_USER} --no-owner --role=${RDS_USER} -n public -d cbdb ./cbdb.dump
    pg_restore -h ${RDS_URL%%:*} -p ${RDS_URL##*:} -U ${RDS_USER} --no-owner --role=${RDS_USER} -n public -d uaadb ./uaadb.dump
    pg_restore -h ${RDS_URL%%:*} -p ${RDS_URL##*:} -U ${RDS_USER} --no-owner --role=${RDS_USER} -n public -d periscopedb ./periscopedb.dump</small></pre>
    
    For more information refer to the [PostgreSQL documentation](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).
    
5. Set the following variables in Cloudbreak Profile file.  
    Modify the RDS_URL parameter by passing the ENDPOINT:PORT of your external database. There is no need to modify values of any other parameters, as their values are derived from the RDS_URL.

    <pre><small>export RDS_URL=localhost:5432
    export RDS_USER=admin
    export RDS_PASS=admin123
    
    export CB_DB_PORT_5432_TCP_ADDR=${RDS_URL%%:*}
    export CB_DB_PORT_5432_TCP_PORT=${RDS_URL##*:}
    export CB_DB_ENV_USER=$RDS_USER
    export CB_DB_ENV_PASS=$RDS_PASS
    export CB_DB_ENV_DB=cbdb
    
    export PERISCOPE_DB_TCP_ADDR=${RDS_URL%%:*}
    export PERISCOPE_DB_TCP_PORT=${RDS_URL##*:}
    export PERISCOPE_DB_USER=$RDS_USER
    export PERISCOPE_DB_PASS=$RDS_PASS
    export PERISCOPE_DB_NAME=periscopedb
    export PERISCOPE_DB_SCHEMA_NAME=public
    
    export IDENTITY_DB_URL=$RDS_URL
    export IDENTITY_DB_USER=$RDS_USER
    export IDENTITY_DB_PASS=$RDS_PASS
    export IDENTITY_DB_NAME=uaadb</small></pre>

6. Restart Cloudbreak application by using the `cbd restart` command. 

[Comment]: <> (How can I verify these steps worked? You can try to kill the local common_db container, the application should be able to continue to run)

