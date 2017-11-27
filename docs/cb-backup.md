### Backing up Cloudbreak database 

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