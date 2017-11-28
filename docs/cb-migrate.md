## Move the Cloudbreak Instance

To transfer a Cloudbreak Server that uses the default embedded PostgreSQL database from one host to a new host, perform these tasks:

1. Back up current data - from the original Cloudbreak database.  
    Refer to [Back up Cloudbreak Database](cb-db.md#back-up-cloudbreak-database)   
2. Launch a new Cloudbreak instance.  
    Refer to [Launch Cloudbreak](index.md#launch-cloudbreak)   
3. [Populate Databases with Information from the Original Server](#populate-databases-with-information-from-the-original-server) - on the new host.  
4. [Modify Cloudbreak Profile](#modify-cloudbreak-profile)  


### Populate Databases with Information from the Original Server

Perform these steps to populate databases with information from the original server.

**Steps**

1. Copy the saved database files from [Back up Cloudbreak Database](cb-db.md#back-up-cloudbreak-database) to the new Cloudbreak server host.

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

**Steps**

1. Ensure that the following parameter values match in the origin and target Profile files and modify Profile file of the target environment, if necessary:

    <pre><small>export UAA_DEFAULT_USER_EMAIL=admin@example.com
    export UAA_DEFAULT_SECRET=cbsecret
    export UAA_DEFAULT_USER_PW=cbuser</small></pre>
    
2. Restart Cloudbreak application by using the `cbd restart` command.  
 
