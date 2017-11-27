## Moving the Cloudbreak instance

To transfer a Cloudbreak Server that uses the default, embedded, PostgreSQL database from one host to a new host:

**Steps**

1. [Back up current data](cb-backup.md) - from the original Cloudbreak database.
2. [Launch the new Cloudbreak instance](index.md#launch-cloudbreak) - on the target host
3. [Populate databases with information from the original Server.](#populate_db)
4. [Modify Cloudbreak Profile](#modify_profile)

#### <a name="populate_db"></a> Populate databases with information from the original server.

1. Copy the dump files to the remote server.

2. Copy the dump files into the database container with the following commands. Please modify the original location as necessary (currently `/tmp`):

    <pre><small>docker cp /tmp/cbdb.dump cbreak_commondb_1:/cbdb.dump
    docker cp /tmp/uaadb.dump cbreak_commondb_1:/uaadb.dump
    docker cp /tmp/periscopedb.dump cbreak_commondb_1:/periscopedb.dump</small></pre>
   
3. Execute `docker stop cbreak_identity_1`

4. Execute `docker exec -it cbreak_commondb_1 bash` command to enter the container of the database, then execute the following commands:
   
    <pre><small>psql -U postgres
    drop database uaadb ;
    drop database cbdb;
    drop database periscopedb;
    create database uaadb ;
    create database cbdb;
    create database periscopedb;</small></pre>
 
    > NOTE: If you receive `ERROR:  database "uaadb" is being accessed by other users` error then please ensure that    cbreak_identity_1 container is not running, then retry dropping uaadb.  

5. Exit the PostgreSQL interactive terminal.
    <pre><small>\q</small></pre> 
     
6. Restore the databases from the original backups:
   
    <pre><small>pg_restore -U postgres -d periscopedb periscopedb.dump
    pg_restore -U postgres -d cbdb cbdb.dump
    pg_restore -U postgres -d uaadb uaadb.dump</small></pre>
   
7. Quit from the container with shortcut `CTRL+d`     

#### <a name="modify_profile"></a> Modify Cloudbreak Profile

1. Please ensure that the following parameter values match in the origin and target Profile files and modifiy Profile file of the target environment, if necessary:

    <pre><small>export UAA_DEFAULT_USER_EMAIL=admin@example.com
    export UAA_DEFAULT_SECRET=cbsecret
    export UAA_DEFAULT_USER_PW=cbuser</small></pre>
    
2. Restart Cloudbreak application by using the `cbd restart` command.  
 
