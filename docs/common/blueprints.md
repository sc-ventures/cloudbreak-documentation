**Cluster blueprints** are your declarative definition of a Hadoop cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. 

You have three options:

* Use one of the pre-defined blueprints.  
* Copy and edit one of the pre-defined blueprints.   
* Add your custom blueprint by uploading a JSON file or pasting the JSON text. 

We recommend that you review the default blueprints to check if they meet your requirements. You can do this by expanding  the **manage bluerints** pane in the Cloudbreak web UI (shown in the screenshot) or by reading the documentation below.  

<a href="../images/cb-blueprints.png" target="_blank" title="click to enlarge"><img src="../images/cb-blueprints.png" width="650" title="Azure Portal"></a> 

[Here](https://raw.githubusercontent.com/sequenceiq/cloudbreak/master/integration-test/src/main/resources/blueprint/multi-node-hdfs-yarn.bp) is an example of a blueprint. 

The host groups in the JSON will be mapped to a set of instances when starting the cluster, and the specified services and components will be installed on the corresponding nodes. It is not necessary to define a complete configuration in the blueprint. If a configuration is missing, Ambari will fill that with a default value. 

A blueprint can be modified later from the Ambari UI.

A blueprint can be exported from a running Ambari cluster and can be reused in Cloudbreak after slight modifications. When a blueprint is exported, some configurations are hardcoded for example domain names, memory configurations, and so on, that won't be applicable to the Cloudbreak cluster. There is no automatic way to modify an exported blueprint and make it instantly usable in Cloudbreak, the modifications have to be done manually.

### Default Blueprints 

Cloudbreak includes three default HDP cluster blueprints:



#### HDP Version: **HDP 2.6**

| Cluster Type  | Services | Description  |
|:------------- |:---|:-------------|
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 1.6,<br>Zeppelin 0.7.0 | This cluster configuration includes Spark 1.6 with Zeppelin. |
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 2.1,<br>Zeppelin 0.7.0 | This cluster configuration includes Spark 2.1 with Zeppelin. |
| EDW - Analytics | <span><i class="fa fa-check" style="color: green"></i> Hive 2 LLAP</span>,<br>Zeppelin 0.7.0 | This cluster configuration includes Hive 2 LLAP. |
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive 1.2.1,<br>Spark 1.6 | This cluster configuration includes Hive and Spark 1.6. |
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive 1.2.1,<br> Spark 2.1 | This cluster configuration includes Hive and Spark 2.1. |
| BI | <span><i class="fa fa-warning" style="color: orange"></i> Druid 0.9.2</span> | This cluster configuration includes a Technical Preview of Druid. |

#### HDP Version: **HDP 2.5**

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

### Copy and Edit Existing Blueprint 

You can modify default or previously added blueprints in the **manage blueprints** tab. To do that, expand the entry in the Cloudbreak UI and then click **copy & edit**. 


### Add Custom Blueprint

For correct blueprint layout, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page.

You can define reusable blueprints for your clusters in the **manage blueprints** tab. 

To add your own blueprint, click **+create blueprint** and enter the following parameters:

| Parameter | Value |
|---|---|
| Name | Enter a name for your blueprint. |
| Description | (Optional) Enter a description for your blueprint.|
| Blueprint Source| <p>Select one of: <ul><li>**Text**: Paste blueprint in JSON format.</li><li> **File**: Upload a file that contains the blueprint.</li><li> **URL**: Specify the URL for your blueprint.</li></ul> |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this blueprint to create clusters, but they cannot delete it. | 

