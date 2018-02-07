## Troubleshooting Cluster Creation

### Cannot Access Oozie Web UI

{!docs/common/extjs-recipe.md!} 


### Quota Limitations

Each cloud provider has quota limitations on various cloud resources, and these quotas can usually be increased on request. If there is an error message in Cloudbreak stating that there are no more available EIPs (Elastic IP Address) or VPCs, you need to request more of these resources. 

To see the limitations visit the cloud provider’s site:

* [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) 
* [Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits)
* [GCP Resource Quotas](https://cloud.google.com/compute/quotas) 

### Connection Timeout When Ports Are Not Open

In the cluster installation wizard, you must specify on which node you want to run the Ambari server. Cloudbreak communicates with this node to orchestrate the installation.

A common reason for connection timeout is security group misconfiguration. Cloudbreak allows configuring different security groups for the different instance groups; however, there are certain requirements for the Ambari server node. Specifically, the following ports must be open in order to communicate with that node:

* 22 (SSH)  
* 9443 (two-way-ssl through nginx) 


### Blueprint Errors 

#### Invalid Services and Configurations

Ambari blueprints are a declarative definition of a cluster. With a blueprint, you specify a stack, the component layout, and the configurations to materialize a Hadoop cluster instance via a REST API without having to use the Ambari cluster install wizard. 

Cloudbreak supports any type of blueprints, which is a common source of errors. These errors are only visible once the core infrastructure is up and running and Cloudbreak tries to initiate the cluster installation through Ambari. Ambari validates the blueprint and  rejects it if it's invalid. 

For example, if there are configurations for a certain service like Hive but Hive as a service is not mapped to any host group, the blueprint is invalid.

To fix these type of issues, edit your blueprint and then reinstall your cluster. Cloudbreak UI has support for this so the infrastructure does not have to be terminated.

There are some cases when Ambari cannot validate your blueprint beforehand. In these cases, the issues are only visible in the Ambari server logs. To troubleshoot, check Ambari server logs.


#### Wrong HDP Version

In the blueprint, only the major and minor HDP version should be defined (for example, "2.6"). If wrong version number is provided, the following error can be found in the logs:

```
5/15/2017 12:23:19 PM testcluster26 - create failed: Cannot use the specified Ambari stack: HDPRepo
{stack='null'; utils='null'}
. Error: org.apache.ambari.server.controller.spi.NoSuchResourceException: The specified resource doesn't exist: Stack data, Stack HDP 2.6.0.3 is not found in Ambari metainfo
```

For correct blueprint layout, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page.
  

### Recipe Errors 

#### Recipe Execution Times Out

If the scripts are taking too much time to execute, the processes will time out, as the threshold for all recipes is set to 15 minutes. To change this threshold, you must override the default value by adding the following to the cbd Profile file:


```
export CB_JAVA_OPTS=” -Dcb.max.salt.recipe.execution.retry=90”
``` 

This property indicates the number of tries for checking if the scripts have finished with a sleep time (i.e. the wait time between two polling attempts) of 10 seconds. The default value is 90. To increase the threshold, provide a number greater than 90. You must restart Cloudbreak after changing properties in the Profile file.


#### Recipe Execution Fails

It often happens that a script cannot be executed successfully because there are typos or errors in the script. To verify this you can check the recipe logs at
`/var/log/recipes`. For each script, there will be a separate log file with the name of the script that you provided on the Cloudbreak UI.