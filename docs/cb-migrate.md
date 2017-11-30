## Moving a Cloudbreak Instance

To transfer a Cloudbreak instance from one host to another, perform these tasks:

1. If you are using the embedded PostgreSQL database, back up current Cloudbreak data. Refer to [Backup Cloudbreak Database](#backup-cloudbreak-database)   
2. Launch a new Cloudbreak instance on a new host. Refer to [Launch Cloudbreak](index.md#launch-cloudbreak)   
3. If you are using the embedded PostgreSQL database, [populate the new Cloudbreak instance database with the dump from the original Cloudbreak instance](#populate-database-with-dump-from-original-cloudbreak-instance) on the new host.  
4. [Modify Cloudbreak Profile](#modify-cloudbreak-profile)  


### Populate Database with Dump from Original Cloudbreak instance

Perform these steps to populate databases with information from the Cloudbreak server.

1. Copy the saved database files from [Backup Cloudbreak Database](#backup-cloudbreak-database) to the new Cloudbreak server host.

2. Copy the dump files into the database container with the following commands. Modify the original location as necessary (currently `/tmp`):

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

1. Ensure that the following parameter values match in the origin and target Profile files and modify Profile file of the target environment, if necessary:

    <pre><small>export UAA_DEFAULT_USER_EMAIL=admin@example.com
    export UAA_DEFAULT_SECRET=cbsecret
    export UAA_DEFAULT_USER_PW=cbuser</small></pre>
    
2. Restart Cloudbreak application by using the `cbd restart` command.  
 

### Backup Cloudbreak Database 

To create a backup of the embedded PostgreSQL database, perform these steps:

1. On your Cloudbreak host machine, execute `docker exec -it cbreak_commondb_1 bash` command to enter
  the container of the database. If it is not running, start the database container by
  using the `docker start cbreak_commondb_1` command.

3. Create three database dumps (cbdb, uaadb, periscopedb):  

    <pre><small>pg_dump -Fc -U postgres cbdb > cbdb.dump
    pg_dump -Fc -U postgres uaadb > uaadb.dump
    pg_dump -Fc -U postgres periscopedb > periscopedb.dump</small></pre>
                
4. Quit from the container with shortcut `CTRL+d`.

5. Save the previously created dumps to the host instance:               

    <pre><small>docker cp cbreak_commondb_1:/cbdb.dump ./cbdb.dump
    docker cp cbreak_commondb_1:/uaadb.dump ./uaadb.dump
    docker cp cbreak_commondb_1:/periscopedb.dump ./periscopedb.dump</small></pre>

