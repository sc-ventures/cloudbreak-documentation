## Moving a Cloudbreak Instance

To transfer a Cloudbreak instance from one host to another, perform these tasks:

1. If you are using the embedded PostgreSQL database, [back up current Cloudbreak database](#back-up-cloudbreak-database) data  
2. Launch a new Cloudbreak instance and start Cloudbreak. Refer to [Launch Cloudbreak](index.md#launch-cloudbreak)   
3. If you are using the embedded PostgreSQL database, [populate the new Cloudbreak instance database with the dump from the original Cloudbreak instance](#populate-database-with-dump-from-original-cloudbreak-instance) on the new host.  
4. [Modify Cloudbreak Profile](#modify-cloudbreak-profile)  

### Back up Cloudbreak Database 

To create a backup of the embedded PostgreSQL database, perform these steps.

**Steps** 

1. On your Cloudbreak host machine, execute the following  command to enter the container of the database:

    <pre>docker exec -it cbreak_commondb_1 bash</pre> 
    If it is not running, start the database container by using the `docker start cbreak_commondb_1` command.

3. Create three database dumps (cbdb, uaadb, periscopedb):  

    <pre><small>pg_dump -Fc -U postgres cbdb > cbdb.dump
    pg_dump -Fc -U postgres uaadb > uaadb.dump
    pg_dump -Fc -U postgres periscopedb > periscopedb.dump</small></pre>
                
4. Quit from the container with shortcut `CTRL+d`.

5. Save the previously created dumps to the host instance:               

    <pre><small>docker cp cbreak_commondb_1:/cbdb.dump ./cbdb.dump
    docker cp cbreak_commondb_1:/uaadb.dump ./uaadb.dump
    docker cp cbreak_commondb_1:/periscopedb.dump ./periscopedb.dump</small></pre>



### Populate Database with Dump from Original Cloudbreak Instance

Perform these steps to populate databases with information from the Cloudbreak server.

**Steps** 

1. Copy the saved database files from [Backup Cloudbreak Database](#backup-cloudbreak-database) to the new Cloudbreak server host.

2. Copy the dump files into the database container with the following commands. Modify the location as necessary (The example below assumes that the files are in `/tmp`):

    <pre><small>docker cp /tmp/cbdb.dump cbreak_commondb_1:/cbdb.dump
    docker cp /tmp/uaadb.dump cbreak_commondb_1:/uaadb.dump
    docker cp /tmp/periscopedb.dump cbreak_commondb_1:/periscopedb.dump</small></pre>
   
3. Execute the following command to stop the container:

    <pre>docker stop cbreak_identity_1</pre>

4. Execute the following command to enter the container of the database:

    <pre>docker exec -it cbreak_commondb_1 bash</pre>

5. Execute the following commands:
   
    <pre><small>psql -U postgres
    drop database uaadb;
    drop database cbdb;
    drop database periscopedb;
    create database uaadb;
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

1. Ensure that the following parameter values match in the origin and target Profile files and modify Profile file of the target environment if necessary:

    <pre><small>export UAA_DEFAULT_USER_EMAIL=admin@example.com
    export UAA_DEFAULT_SECRET=cbsecret
    export UAA_DEFAULT_USER_PW=cbuser</small></pre>
    
2. Restart Cloudbreak application by using the `cbd restart` command.  
 
    After performing these steps the migration is complete. To verify, log in to the UI of your new Cloudbreak instance and make sure that it contains the information from your old instance.  

