
## Local Development 

The following documentation will help you set up your local development environment on Mac OS X.

### Prerequisites
   
To use this development environment on Mac OS X, you need the following:

* **Brew**: Follow the install instructions on the [brew homepage](https://brew.sh).

* **Java and Docker**: You can get these with the following commands:

    <pre>
brew cask install java
brew install docker
brew install docker-machine
brew install boot2docker
</pre>

* **Hypervisor**: Cloudbreak deployer has built-in xhyve setup option, but some of us use VirtualBox instead (and so do the docker docs). Cloudbreak deployer works with both; the choice is up to you.

    To set up xhyve:
    
    <pre>
brew install docker-machine-driver-xhyve
</pre>

    To set up VirtualBox:
    
    <pre>
brew cask install virtualbox
</pre>


### Set up Cloudbreak Deployer 
 
The simplest way to prepare the working environment is to start the Cloudbreak application on your local machine using [Cloudbreak deployer](https://github.com/hortonworks/cloudbreak-deployer).

>>>>TO-DO: Is the sentence above a step or just an overview of this topic? If this a step, what exactly do I need to do? I think the repo is copied in step 2 below so I assume that this is not a step. 

1. Create a directory which will store the necessary configuration files and dependencies of the Cloudbreak deployer. This directory must be created outside of the cloned Cloudbreak deployer git repository:

    <pre>
mkdir cbd-local
cd cbd-local</pre>

2. Get the cloudbreak-deployer onto your machine:

    <pre>
curl https://raw.githubusercontent.com/hortonworks/cloudbreak-deployer/master/install-dev | sh [-s branch] && cbd --version
</pre>
    
    Add the `-s` branch option for `sh` in case you'd like to checkout a branch different than master.

3. Next, you must set up your docker-machinep. Depending on whether you chose to use xhyve or VirtualBox, type the commands listed below. 

    xhyve:
    
    <pre>
cbd machine create
eval $(docker-machine env cbd)
</pre>

    VirtualBox:

    <pre>
docker-machine create --driver virtualbox --virtualbox-disk-size "50000" cbd
eval $(docker-machine env cbd)
</pre>

4. IP settings are based on your docker-machine configuration. Use the `docker-machine ip cbd` command to print the IP address of your docker machine. This is the address which you should use in multiple places below. Let's refer to this address as "YOUR_IP" throughout this document: `YOUR_IP=$(docker-machine ip cbd)`.
</pre>

    >>>>TO-DO: Is the `YOUR_IP=$(docker-machine ip cbd)` part just describing a convention that we want to use or can you actually enter this anywhere?     

5. Create a file called `Profile` in the cbd-local directory that you just created, and, depending on whether you are using xhyve or VirtualBox, add the content listed below. 

    The CB_SCHEMA_SCRIPTS_LOCATION environment variable configures the location of SQL scripts that are in the 'core/src/main/resources/schema' directory in the cloned Cloudbreak git repository. Note that the full path needs to be configured and environment variables such as `$USER` cannot be used. You must also set a password for your local Cloudbreak in UAA_DEFAULT_USER_PW:


    xhyve:

    <pre>
export PRIVATE_IP=$PUBLIC_IP
export ULU_SUBSCRIBE_TO_NOTIFICATIONS=true
export CB_INSTANCE_UUID=$(uuidgen | tr '[:upper:]' '[:lower:]')
export CB_SCHEMA_SCRIPTS_LOCATION=/Users/YOUR_USERNAME/YOUR_PROJECT_DIR/cloudbreak/core/src/main/resources/schema
export UAA_DEFAULT_USER_PW=YOUR_PASSWORD
</pre>

    VirtualBox:

    <pre>
export PUBLIC_IP=$(docker-machine ip cbd)
export DOCKER_MACHINE=cbd
export PRIVATE_IP=$PUBLIC_IP
export ULU_SUBSCRIBE_TO_NOTIFICATIONS=true
export CB_INSTANCE_UUID=$(uuidgen | tr '[:upper:]' '[:lower:]')
export CB_SCHEMA_SCRIPTS_LOCATION=/Users/YOUR_USERNAME/YOUR_PROJECT_DIR/cloudbreak/core/src/main/resources/schema
export UAA_DEFAULT_USER_PW=YOUR_PASSWORD
</pre>

6. Next, run these commands to start Cloudbreak and show the Cloudbreak logs:

    <pre>
cbd start
cbd logs cloudbreak
</pre>

    If you see org.apache.ibatis.migration.MigrationException at the end of the logs, run the following commands to fix the DB.

    <pre>
cbd migrate cbdb up
cbd migrate cbdb pending
</pre>

    Next, rerun `cbd start` and `cbd logs cloudbreak` commands.

7. After performing these steps, Cloudbreak is available at http://YOUR_IP. For more details and config parameters check the documentation for [Cloudbreak Deployer](https://github.com/hortonworks/cloudbreak-deployer).

    The deployer generated a `certs` directory under the `cbd-local` directory. You will need it later during the IDEA setup.

    In order to kill Cloudbreak container running in docker/boot2docker and redirect the Cloudbreak related traffic to the Cloudbreak running in IDEA use the followig command:
    
    <pre>
cbd util local-dev
</pre>

### Set up IDEA

#### Checkout the Cloudbreak Repository

Go to https://github.com/hortonworks/cloudbreak, clone or download, Use SSH which is described here: https://help.github.com/articles/connecting-to-github-with-ssh/

>>>>TO-DO: What do you do 

#### Project Settings in IDEA

In IDEA set your SDK to your Java version under:
Configure -> Project Defaults -> Project Structure -> Project SDK

Cloudbreak can be imported into IDEA as gradle project by specifying the cloudbreak repo root under Import Project. Once it is done, you need to import the proper code formatter by using the __File -> Import Settings...__ menu and selecting the `idea_settings.jar` located in the `config/idea` directory in Cloudbreak git repository.

To launch the Cloudbreak application execute the `com.sequenceiq.cloudbreak.CloudbreakApplication` class with VM options:
```
-XX:MaxPermSize=1024m
-Dcb.cert.dir=FULL_PATH_OF_THE_CERTS_DIR_GENERATED_BY_CBD
-Dcb.client.id=cloudbreak
-Dcb.client.secret=CB_SECRET_GENERATED_BY_CBD
-Dcb.db.port.5432.tcp.addr=YOUR_IP
-Dcb.db.port.5432.tcp.port=5432
-Dcb.identity.server.url=http://YOUR_IP:8089
-Dserver.port=9091
-Dcb.schema.migration.auto=true
```

The `-Dcb.cert.dir=FULL_PATH_OF_THE_CERTS_DIR_GENERATED_BY_CBD` value above needs to be replaced with the full path of `certs` directory generated by the Cloudbreak-Deployer e.g. `/Users/YOUR_USERNAME/YOUR_PROJECT_DIR/cbd-local/certs`.

The `-Dcb.client.secret=CB_SECRET_GENERATED_BY_CBD` value has to be replaced with the value of UAA_DEFAULT_SECRET from the cdb-local/Profile file.
The databse migration is ran automatically by Cloudbreak, this automated migration could be turn off through the `-Dcb.schema.migration.auto=false` VM option.

### Set up Command line

To run Cloudbreak from command line you have to list the JVM parameters from above for gradle:

```
./gradlew :core:bootRun -PjvmArgs="-Dcb.cert.dir=FULL_PATH_OF_THE_CERTS_DIR_GENERATED_BY_CBD \
-Dcb.client.id=cloudbreak \
-Dcb.client.secret=CB_SECRET_GENERATED_BY_CBD \
-Dcb.db.port.5432.tcp.addr=YOUR_IP \
-Dcb.db.port.5432.tcp.port=5432 \
-Dcb.identity.server.url=http://YOUR_IP:8089 \
-Dserver.port=9091"
```

Same as with IDEA the `-Dcb.cert.dir=FULL_PATH_OF_THE_CERTS_DIR_GENERATED_BY_CBD` value above needs to be replaced with the full path of `certs` directory generated by the Cloudbreak-Deployer e.g. `/Users/YOUR_USERNAME/YOUR_PROJECT_DIR/cbd-local/certs`.

The `-Dcb.client.secret=CB_SECRET_GENERATED_BY_CBD` value has to be replaced with the value of UAA_DEFAULT_SECRET from the cdb-local/Profile file.
The databse migration is ran automatically by Cloudbreak, this automated migration could be turn off through the `-Dcb.schema.migration.auto=false` VM option.

### Set up Database Development

If schema change is required in Cloudbreak database (cbdb), then the developer needs to write SQL scripts to migrate the database accordingly. In Cloudbreak the schema migration is managed by [MYBATIS Migrations](https://github.com/mybatis/migrations) and the cbd tool provides an easy-to-use wrapper for it. The syntax for using the migration commands is `cbd migrate <database name> <command> [parameters]` e.g. `cbd migrate migrate status`.

Create a SQL template for schema changes:
```
cbd migrate cbdb new "CLOUD-123 schema change for new feature"
```
As as result of the above command an SQL file template is generated under the path specified in `CB_SCHEMA_SCRIPTS_LOCATION` environment variable, which is defined in Profile. The structure of the generated SQL template looks like the following:
```
-- // CLOUD-123 schema change for new feature
-- Migration SQL that makes the change goes here.



-- //@UNDO
-- SQL to undo the change goes here.
```
Once you have implemented your SQLs then you can execute them with:
```
cbd migrate cbdb up
```
Make sure pending SQLs to run as well:
```
cbd migrate cbdb pending
```
If you would like to rollback the last SQL file, then just use the down command:
```
cbd migrate cbdb down
```
On order to check the status of database
```
cbd migrate cbdb status

#Every script that has not been executed will be marked as ...pending... in the output of status command:

------------------------------------------------------------------------
-- MyBatis Migrations - status
------------------------------------------------------------------------
ID             Applied At          Description
================================================================================
20150421140021 2015-07-08 10:04:28 create changelog
20150421150000 2015-07-08 10:04:28 CLOUD-607 create baseline schema
20150507121756 2015-07-08 10:04:28 CLOUD-576 change instancegrouptype hostgroup to core
20151008090632    ...pending...    CLOUD-123 schema change for new feature

------------------------------------------------------------------------
```

### Building

Gradle is used for build and dependency management. Gradle wrapper is added to Cloudbreak git repository, therefore building can be done with:

<pre>
./gradlew clean build
</pre>
