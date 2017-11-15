## Blueprints

**Ambari blueprints** are your declarative definition of a Hadoop cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. 

You have three options concerning using blueprints with Cloudbreak:

* Use one of the pre-defined blueprints.    
* Add your custom blueprint by uploading a JSON file or pasting the JSON text. 

We recommend that you review the default blueprints to check if they meet your requirements. You can do this by selecting **Blueprints** from the navigation pane in the Cloudbreak web UI or by reading the documentation below.
  

### Use Default Blueprints 

To use one of the default blueprints, simply select them when creating a cluster. The option is available on the **General Configuration** page. First select the **Stack Version** and then select your chosen blueprint under **Cluster Type**. 


#### Default Blueprints 

Cloudbreak includes the following default HDP cluster blueprints:

Platform Version: **HDP 2.6**

| Cluster Type  | Main Services | List of All Services Included |
|:------------- |:---|:-------------|
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 1.6,<br>Zeppelin 0.7.0 | HDFS, YARN, MapReduce2, Tez, Hive 1.2.1, Pig, Sqoop, ZooKeeper, Ambari Metrics, Spark 1.6, Zeppelin 0.7.0 |
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 2.1,<br>Zeppelin 0.7.0 | HDFS, YARN, MapReduce2, Tez, Hive 1.2.1, Pig, Sqoop, ZooKeeper, Ambari Metrics, Spark 2.1, Zeppelin 0.7.0 |
| EDW - Analytics | <span><i class="fa fa-check" style="color: green"></i> Hive 2 LLAP</span>,<br>Zeppelin 0.7.0 | HDFS, YARN, MapReduce2, Tez, Hive 2 LLAP, Druid, Pig, ZooKeeper, Ambari Metrics, Spark 2.1 |
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive 1.2.1,<br>Spark 1.6 | HDFS, YARN, MapReduce2, Tez, Hive 1.2.1, Pig, Sqoop, ZooKeeper, Ambari Metrics, Spark 1.6, Druid 0.9.2 |
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive 1.2.1,<br> Spark 2.1 | HDFS, YARN, MapReduce2, Tez, Hive 1.2.1, Pig, ZooKeeper, Ambari Metrics, Spark 2.1 |
| BI | <span><i class="fa fa-warning" style="color: orange"></i> Druid 0.9.2</span> | HDFS, YARN, MapReduce2, Tez, Druid, Sqoop, ZooKeeper, Ambari Metrics |



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


### Add Custom Blueprint

This option allows you to save your custom blueprints. For correct blueprint layout and other useful information about Ambari blueprints, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page.

<div class="note">
    <p class="first admonition-title">Supported Ambari and HDP Versions</p>
    <p class="last">
Cloudbreak supports the following Ambari and HDP versions:<ul><li>Ambari <b>2.5.x</b></li><li>HDP <b>2.6.x</b> and HDP <b>2.5.x</b></li></ul>Ambari 2.6.x is not supported.
</p>
</div>

#### Creating a Blueprint

Ambari blueprints are specified in the JSON format. After you provide the blueprint to Cloudbreak, the host groups in the JSON will be mapped to a set of instances when starting the cluster, and the specified services and components will be installed on the corresponding nodes. It is not necessary to define a complete configuration in the blueprint. If a configuration is missing, Ambari will fill that with a default value. 
Furthermore, a blueprint can be modified later from the Ambari UI.

A blueprint can be exported from a running Ambari cluster and can be reused in Cloudbreak after slight modifications. When a blueprint is exported, some configurations are hardcoded for example domain names, memory configurations, and so on, that won't be applicable to the Cloudbreak cluster. There is no automatic way to modify an exported blueprint and make it instantly usable in Cloudbreak, the modifications have to be done manually.

#### Example Blueprints

[comment]: <> (TO-DO: Maybe we can find some newer examples?)

Here are some [blueprint examples](https://github.com/hortonworks/cloudbreak/tree/master/core/src/main/resources/defaults/blueprints).  

[comment]: <> ( More blueprints (HDP 2.4) )
[comment]: <> ( [Smallest Possible HDP 2.4](https://raw.githubusercontent.com/sequenceiq/cloudbreak/master/integration-test/src/main/resources/blueprint/multi-node-hdfs-yarn.bp)
[comment]: <>  ( [Small HDP 2.4](https://raw.githubusercontent.com/hortonworks/cloudbreak/master/core/src/main/resources/defaults/blueprints/hdp-small-default.bp)
[comment]: <>  ( [Small HDP 2.4 Streaming](https://raw.githubusercontent.com/hortonworks/cloudbreak/master/core/src/main/resources/defaults/blueprints/hdp-streaming-cluster.bp)
[comment]: <>   ( [Small HDP 2.4 Spark](https://raw.githubusercontent.com/hortonworks/cloudbreak/master/core/src/main/resources/defaults/blueprints/hdp-spark-cluster.bp)


#### Upload a Blueprint 

Once you have your blueprint ready, perform these steps.

**Steps**

1. In the Cloudbreak UI, select **Blueprints** from the navigation pane. 
2. To add your own blueprint, click **Create Blueprint** and enter the following parameters:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your blueprint. |
| Description | (Optional) Enter a description for your blueprint.|
| Blueprint Source | <p>Select one of: <ul><li>**Text**: Paste blueprint in JSON format.</li><li> **File**: Upload a file that contains the blueprint.</li><li> **URL**: Specify the URL for your blueprint.</li></ul> |

    <a href="../images/cb-blueprint-add.png" target="_blank" title="click to enlarge"><img src="../images/cb-blueprint-add.png" width="650" title="Cloudbreak web UI"></a> 

2. To use the uploaded blueprints, select it when creating a cluster. The option is available on the **General Configuration** page. First select the **Platform Version** and then select your chosen blueprint under **Cluster Type**. 

    <a href="../images/cb-blueprint-select.png" target="_blank" title="click to enlarge"><img src="../images/cb-blueprint-select.png" width="650" title="Cloudbreak web UI"></a> 



### View Blueprint Details 

Once a blueprint has been registered in Cloudbreak, you can access its details in the Cloudbreak UI.

**Steps**

1. In the Cloudbreak UI, select **Blueprints** from the navigation pane. 

2. Click on an entry to navigate to details. 

    You can view blueprint details using the **List View** and **Raw View**:

    <a href="../images/cb-blueprint-details.png" target="_blank" title="click to enlarge"><img src="../images/cb-blueprint-details.png" width="650" title="Cloudbreak web UI"></a> 


 
### Delete Blueprint

To delete a default or custom blueprint, perform these steps.

**Steps**

1. In the Cloudbreak UI, select **Blueprints** from the navigation pane. 

2. Click on an entry to navigate to details. 

3. Click **Delete**. 

3. Confirm delete. 



### Modifying Existing Blueprints

You cannot directly modify default blueprints.

Similarly, you cannot directly modify the custom blueprints that you have uploaded or pasted. However, If you provided a URL to the location where the blueprint is stored, in order to modify the blueprint simply update it in the location to which the URL is pointing.       


