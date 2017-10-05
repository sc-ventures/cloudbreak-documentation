## Blueprints

**Ambari blueprints** are your declarative definition of a Hadoop cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. 

You have three options concerning using blueprints with Cloudbreak:

* Use one of the pre-defined blueprints.    
* Add your custom blueprint by uploading a JSON file or pasting the JSON text. 
* Copy and edit one of the pre-defined blueprints. 

We recommend that you review the default blueprints to check if they meet your requirements. You can do this by expanding  the **manage bluerints** pane in the Cloudbreak web UI or by reading the documentation below.
  

### Use Default Blueprints 

To use one of the default blueprints, simply select them when creating a cluster. The option is available on the **General Configuration** page. First select the **Stack Version** and then select your chosen blueprint under **Cluster Type**.  

#### Default Blueprints 

Cloudbreak includes the following default HDP cluster blueprints:


##### HDP Version: **HDP 2.6**

| Cluster Type  | Services | Description  |
|:------------- |:---|:-------------|
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 1.6,<br>Zeppelin 0.7.0 | This cluster configuration includes Spark 1.6 with Zeppelin. |
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 2.1,<br>Zeppelin 0.7.0 | This cluster configuration includes Spark 2.1 with Zeppelin. |
| EDW - Analytics | <span><i class="fa fa-check" style="color: green"></i> Hive 2 LLAP</span>,<br>Zeppelin 0.7.0 | This cluster configuration includes Hive 2 LLAP. |
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive 1.2.1,<br>Spark 1.6 | This cluster configuration includes Hive and Spark 1.6. |
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive 1.2.1,<br> Spark 2.1 | This cluster configuration includes Hive and Spark 2.1. |
| BI | <span><i class="fa fa-warning" style="color: orange"></i> Druid 0.9.2</span> | This cluster configuration includes a Technical Preview of Druid. |


##### HDP Version: **HDP 2.5**

| Cluster Type  | Services | Description  |
|:------------- |:---|:-------------|
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 1.6,<br>Zeppelin 0.6.0 | This cluster configuration includes Spark 1.6 and Zeppelin. |
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive 1.2.1,<br>Spark 1.6 | This cluster configuration includes Hive and Spark 1.6. |
| EDW - ETL | <i class="fa fa-warning" style="color: orange"></i> Hive 1.2.1,<br> Spark 2.0 | This cluster configuration includes a Technical Preview of Spark 2.0. |
| EDW - Analytics | <span><i class="fa fa-warning" style="color: orange"></i> Hive 2 LLAP</span>,<br>Zeppelin 0.6.0 | This cluster configuration includes a Technical Preview of Hive 2 LLAP. |


<div class="note">
    <p class="first admonition-title">Choosing Your Configuration</p>
    <p class="last">
When creating a cluster, you can choose a more stable cluster configuration for a predicable experience.
Alternatively, you can try the latest capabilities by choosing a cluster configuration
that is much more experimental. The following configuration classification applies:
<ul>
<li><i class="fa fa-check" style="color: green"></i> Stable configurations are the best choice if you want to avoid issues and other problems with launching and using clusters.</li>
<li><i class="fa fa-warning" style="color: orange"></i> If you want to use a Technical Preview version of a component in a release of HDP, use these configurations.</li>
<li><i class="fa fa-warning" style="color: red"></i> These are the most cutting edge of the configurations, including Technical Preview components in a Technical Preview HDP release.</li>
</ul>
</p>
</div>


The following services are included in the respective blueprints:

##### HDP Version: **HDP 2.6**

| Service | Data Science<br>(Spark 1.6) | Data Science<br>(Spark 2.1) | EDW-ETL<br>(Spark 1.6) | EDW-ETL<br>(Spark 2.1) | EDW-Analytics | BI-Druid |
|---|---|---|---|---|---|---|
| HDFS 		 		| x | x | x | x | x | x |
| YARN 			 	| x | x | x | x | x | x |
| MapReduce2  		| x | x | x | x | x | x |
| Tez 					| x | x | x | x | x | x |
| Hive 1.2.1 	 		| x | x | x | x |   |   |
| Hive 2 LLAP 		|   |   |   |   | x |   |
| Druid 		 		|   |   |   |   |   | x |
| Pig  			 	| x | x | x | x | x |   |
| Sqoop 				| x | x | x |   |   | x |
| ZooKeeper 			| x | x | x | x | x | x |
| Ambari Metrics 	| x | x | x | x | x | x |
| Spark 1.6 			| x |   | x |   | x |   |
| Spark 2.1 			|   | x |   | x |   |   |
| Zeppelin 0.7.0 	| x | x |   |   | x |   |
| Slider 				|   |   |   |   | x |   |

##### HDP Version: **HDP 2.5**

| Service | Data Science | EDW-ETL (Spark 1.6) | EDW-ETL (Spark 2.0) | EDW-Analytics |
|---|---|---|---|---|
| HDFS 				| x | x | x | x |
| YARN 				| x | x | x | x |
| MapReduce2  		| x | x | x | x |
| Tez 					| x | x | x | x |
| Hive 1.2.1 			| x | x | x |   | 
| Hive 2 LLAP 		|   |   |   | x | 
| Pig  				| x | x | x | x |
| Sqoop 				| x | x |   |   |
| ZooKeeper 			| x | x | x | x |
| Ambari Metrics 	| x | x | x | x |
| Spark 1.6 			| x | x |   | x |
| Spark 2.0 			|   |   | x |   |
| Zeppelin 0.6.0 	| x |   |   | x |
| Slider 				|   |   |   | x |


### Add Custom Blueprint

This option allows you to save your custom blueprints. For correct blueprint layout and other useful information about Ambari blueprints, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page.

#### Example Blueprint

[Here](https://raw.githubusercontent.com/sequenceiq/cloudbreak/master/integration-test/src/main/resources/blueprint/multi-node-hdfs-yarn.bp) is an example of a blueprint. 

#### Creating a Blueprint

Ambari blueprints are specified in the JSON format. After you provide the blueprint to Cloudbreak, the host groups in the JSON will be mapped to a set of instances when starting the cluster, and the specified services and components will be installed on the corresponding nodes. It is not necessary to define a complete configuration in the blueprint. If a configuration is missing, Ambari will fill that with a default value. 
Furthermore, a blueprint can be modified later from the Ambari UI.

A blueprint can be exported from a running Ambari cluster and can be reused in Cloudbreak after slight modifications. When a blueprint is exported, some configurations are hardcoded for example domain names, memory configurations, and so on, that won't be applicable to the Cloudbreak cluster. There is no automatic way to modify an exported blueprint and make it instantly usable in Cloudbreak, the modifications have to be done manually.

#### Upload a Blueprint 

Once you have your blueprint ready:

1. Navigate to the **manage blueprints** tab. To add your own blueprint, click **+create blueprint** and enter the following parameters:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your blueprint. |
| Description | (Optional) Enter a description for your blueprint.|
| Blueprint Source| <p>Select one of: <ul><li>**Text**: Paste blueprint in JSON format.</li><li> **File**: Upload a file that contains the blueprint.</li><li> **URL**: Specify the URL for your blueprint.</li></ul> |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this blueprint to create clusters, but they cannot delete it. | 

2. To use the uploaded blueprints, simply select it when creating a cluster. The option is available on the **General Configuration** page. First select the **Stack Version** and then select your chosen blueprint under **Cluster Type**. 


### Copy and Edit Existing Blueprint 

You can reuse default and previously added blueprints by using the **copy & edit** option, which allows you to clone and edit an existing blueprint without changing or deleting the original blueprint. To do this: 

1. Navigate to the **manage blueprints** tab, expand the blueprint entry, and then click **copy & edit**.

2. Make updates in your blueprint and save them as a new blueprint.

### Modify Existing Blueprint

To modify existing an blueprint without keeping the original:

* If you pasted or uploaded your blueprint in the Cloudbreak UI, in order to modify it you must delete the entry and add the blueprint again in a new entry.     
* If you provided a URL to the location where the blueprint is stored, in order to modify the blueprint simply update it in the location to which the URL is pointing. 

You can also [copy and edit an existing blueprint](#copy-and-edit-existing-blueprint).

### Delete Blueprint

You can delete previously added items by selecting and item and using the **delete** option. 

>>>>TO-DO: Is it possible to delete default blueprints? 
