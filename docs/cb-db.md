
## Manage Cloudbreak Database

By default, Cloudbreak uses a built-in PostgreSQL database to persist data. For production environments, we suggest that you use an external database, an RDS served by your cloud provider. However, if you choose to use the default database, you should know that Cloudbreak deployer includes features for dumping and restoring built-in databases.


### Dump and Restore Database 

Cloudbreak deployer uses Docker for the underlying infrastructure and uses Docker volume for storing data. There are two separate volumes: 

* a volume called `common` for storing live data  
* a volume called `cbreak_dump` for database dumps 

You can override default live data volume any time by extending your `Profile` with the following variable:

```
export COMMON_DB_VOL="my-live-data-volume"
```

To create database dumps, execute the following commands:

```
cbd db dump common cbdb
cbd db dump common uaadb
cbd db dump common periscopedb
```

The dump command has an optional third parameter, the `name` of the dump. If you give your dump a name, Cloudbreak deployer will create a symbolic link which points to the SQL dump. For example: 

```
cbd db dump common cbdb name-of-the-dump
```

To list existing dumps, execute the `cbd db list-dumps` command. Each kind of database dump (cbdb, uaadb, periscopedb) has a link to the latest dump on the `cbreak_dump` volume. During the restore process, Cloudbreak deployer restores from latest dump. To check which dump is the latest, execute:

```
docker run --rm -v cbreak_dump:/dump -it alpine ls -lsa /dump/cbdb/latest
```

To set any of the existing dumps as latest use the `set-dump` command. You can set both regular or named dumps. For example: 

```
cbd db set-dump cbdb 20170628_1805
```
or
```
cbd db set-dump cbdb name-of-the-dump
```

To remove the existing `common` volume, stop all the related Cloudbreak containers with `cbd kill` command, and then remove the volume:

```
docker volume rm common
```

To restore databases from dumps, execute:

```
cbd db restore-volume-from-dump common cbdb
cbd db restore-volume-from-dump common uaadb
cbd db restore-volume-from-dump common periscopedb
```

To save your dumps to the host machine, execute:

```
docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/cbdb/latest/dump.sql > cbdb.sql
docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/uaadb/latest/dump.sql > uaadb.sql
docker run --rm -v cbreak_dump:/dump -it alpine cat /dump/periscopedb/latest/dump.sql > periscopedb.sql
```
