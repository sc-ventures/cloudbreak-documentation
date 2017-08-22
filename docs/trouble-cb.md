
## Troubleshooting Cloudbreak

### Checking the Logs

When troubleshooting, you can access the following Cloudbreak logs.

#### Cloudbreak Logs

When installing Cloudbreak using a pre-built cloud image, the  Cloudbreak Deployer location and the cbd root folder is `/var/lib/cloudbreak-deployment`. You must execute all cbd actions from the cbd root folder as a cloudbreak user. 

> Your cbd root directory may be different if you installed Cloudbreak on your own VM. 

Cloudbreak consists of multiple microservices deployed into Docker containers. 

**Aggregated Logs**

To check aggregated service logs, use the following commands:

`cbd logs` shows all service logs.

`cbd logs | tee cloudbreak.log` allows you to redirect the input into a file for sharing these logs.

**Individual Service Logs**

To check individual service logs, use the following commands:

`cbd logs cloudbreak` shows Cloudbreak logs. This service is the backend service that handles all deployments.

`cbd logs uluwatu` shows Cloudbreak UI logs. Uluwatu is the UI component of Cloudbreak.

`cbd logs identity` shows Identity logs. Identity is responsible for authentication and authorization.

`cbd logs periscope` shows Periscope logs. Periscope is responsible for triggering autoscaling rules.

**Docker Logs**

The same logs can be accessed via Docker commands:

`docker logs cbreak_cloudbreak_1` shows the same logs as `cbd logs cloudbreak`.

Cloudbreak logs are rotated and can be accessed later from the Cloudbreak deployment folder. Each time you restart the application via cbd restart a new log file is created with a timestamp in the name (for example, cbreak-20170821-105900.log). 

> There is a symlink called `cbreak.log` which points to the latest log file. Sharing this symlink does not share the log itself.
 

#### Saltstack Logs

Cloudbreak uses Saltstack to install Ambari and the necessary packages for the HDP provisioning. Salt Master always runs alongside the Ambari Server node. Each instance in the cluster runs a Salt Minion, which connects to the Salt Master. There can be multiple Salt Masters if the cluster is configured to run in HA (High Availability) mode and in this case each Salt Minion connects to each Salt Master.

Cloudbreak also uses SaltStack to execute user-provided customization scripts called "recipes". 

Salt Master and Salt Minion logs can be found at the following location: `/var/log/salt`


#### Ambari Logs

Cloudbreak uses Ambari to orchestrate the installation of the different HDP components. Each instance in the cluster runs an Ambari agent which connects to the Ambari server. Ambari server is declared by the user during the cluster installation wizard. 

**Ambari Server Logs**

Ambari server logs can be found on the nodes where Ambari server is installed in the following locations:

`/var/log/ambari-server/ambari-server.log`

`/var/log/ambari-server/ambari-server.out`

Both files contain important information about the root cause of a certain issue so it is advised to check both.

**Ambari Agent Logs**

Ambari agent logs can be found on the nodes where Ambari agent is installed in the following locations:

`/var/log/ambari-agent/ambari-agent.log`


#### Recipe Logs

Cloudbreak supports "recipes" - user-provided customization scripts that can be run prior to or after cluster installtion. It is the user’s responsibility to provide an idempotent well tested script. If the execution fails, the recipe logs can be found at `/var/log/recipes` on the nodes on which the recipes were executed.

It is advised, but not required to have an advanced logging mechanism in the script, as Cloudbreak always logs every script that are run. Recipes are often the sources of installation failures as users might try to remove necessary packages or reconfigure services.


### Common Errors

#### Quota Limitations

Each cloud provider has quota limitations on various cloud resources, and these quotas can usually be increased on request. If there is an error message in Cloudbreak saying that there are no more available EIPs (Elastic IP Address) or VPCs, you need to request more of these resources. 

To see the limitations visit the cloud provider’s site:

* [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) 
* [Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits)
* [GCP Resource Quotas](https://cloud.google.com/compute/quotas) 

#### Connection Timeout: Ports Not Open

In the cluster installation wizard, you must specify on which node you want to run the Ambari server. Cloudbreak communicates with this node to orchestrate the installation.

A common reason for connection timeout is security group misconfiguration. Cloudbreak allows configuring different security groups for the different instance groups; however, there are certain requirements for the Ambari server node. Specifically, the following ports must be open in order to communicate with that node:

* 22 (SSH)  
* 9443 (two-way-ssl through nginx) 

#### Blueprints: Invalid Services and Configurations

Ambari blueprints are a declarative definition of a cluster. With a blueprint, you specify a stack, the component layout, and the configurations to materialize a Hadoop cluster instance via a REST API without having to use the Ambari cluster install wizard. 

Cloudbreak supports any type of blueprints, which is a common source of errors. These errors are only visible once the core infrastructure is up and running and Cloudbreak tries to initiate the cluster installation through Ambari. Ambari validates the blueprint and  rejects it if it's invalid. 

For example, if there are configurations for a certain service like Hive but Hive as a service is not mapped to any host group, the blueprint is invalid.

To fix these type of issues, edit your blueprint and then reinstall your cluster. Cloudbreak UI has support for this so the infrastructure does not have to be terminated.

There are some cases when Ambari cannot validate your blueprint beforehand. In these cases, the issues are only visible in the Ambari server logs. To trubleshoot, check Ambari server logs.


#### Blueprints: High Availability

Cloudbreak always tries to validate that a blueprint not to include multiple master services into different host groups. However, this exact setup is required for HA clusters. To overcome this, you can disable blueprint validation in the UI (using an advanced option in the Create Cluster wizard > Choose Blueprint), but you must include the necessary configurations.


#### Blueprints: Wrong HDP Version

In the blueprint, only the major and minor HDP version should be defined (for example, "2.6"). If wrong version number is provided, the following error can be found in the logs:

```
5/15/2017 12:23:19 PM testcluster26 - create failed: Cannot use the specified Ambari stack: HDPRepo
{stack='null'; utils='null'}
. Error: org.apache.ambari.server.controller.spi.NoSuchResourceException: The specified resource doesn't exist: Stack data, Stack HDP 2.6.0.3 is not found in Ambari metainfo
```

For correct blueprint layout, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page.
  

#### Recipes: Recipe Execution Times Out

If the scripts are taking too much time to execute, the processes will time out, as the threshold for all recipes is set to 15 minutes. To change this threshold, you must override the default value by adding the following to the cbd Profile file:


```
export CB_JAVA_OPTS=” -Dcb.max.salt.recipe.execution.retry=90”
``` 

This property indicates the number of tries for checking if the scripts have finished with a sleep time (i.e. the wait time between two polling attempts) of 10 seconds. The default value is 90. To increase the threshold, provide a number greater than 90. You must restart Cloudbreak after changing properties in the Profile file.



#### Recipes: Recipe Execution Fails

It often happens that a script cannot be executed successfully because there are typos or errors in the script. To verify this you can check the recipe logs at
`/var/log/recipes`. For each script, there will be a separate log file with the name of the script that you provided on the Cloudbreak UI.


#### Invalid PUBLIC_IP in CBD Profile

The `PUBLIC_IP` property must be set in the cbd Profile file or else you won’t be able to log in on the Cloudbreak UI. 

If you are migrating your instance, make sure that after the start the IP remains valid. If you need to edit the `PUBLIC_IP` property in Profile, make sure to restart Cloudbreak using `cbd restart`.
 

#### Changing Properties in the Cloudbreak Profile File

There are many properties that can be changed in the Cloudbreak application. These values must be changed in the Cloudbreak `Profil`e file. To see all possible options, use the following command:
`cbd env show`.

After changing a property, you must regenerate the config file and restart the application. There are two ways to do this:

In version 1.4.0 and newer of the cbd command line, you can regenerate the config file and restart the application with a single command:

`cbd restart` - same as cbd regenerate/kill/start.

In versions earlier than 1.4.0, you must run the following three commands:

`cbd regenerate` regenerates the Docker compose file
`cbd kill` removes all Docker containers (there is no stop command for this).
`cbd start` starts the application with the new compose file.

#### Changing Amari Credentials

In Cloudbreak 1.14 and later, Cloudbreak creates a new admin user in Ambari, so you can change the credentials of the admin user in the Ambari web UI. You can also set the admin user credentials in the cluster installation wizard in the Cloudbreak UI.

In Cloudbreak versions earlier than 1.14 it is not possible to change the password in the Ambari UI.  If you change the admin credentials in the Ambari UI, Cloudbreak will no longer be able to orchestrate Ambari. To change the password, you must use the option available on the cluster details page in the Cloudbreak UI.



