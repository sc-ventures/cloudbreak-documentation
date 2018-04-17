## Creating custom scripts (recipes)

Although Cloudbreak lets you provision clusters in the cloud based on custom Ambari blueprints, Cloudbreak provisioning options don't consider all possible use cases. For that reason, we introduced recipes. 

A recipe is a script that runs on all nodes of a selected node group at a specific time. You can use recipes for tasks such as installing additional software or performing advanced cluster configuration. For example, you can use a recipe to put a JAR file on the Hadoop classpath.

Available recipe execution times are:  

* Before Ambari server start    
* After Ambari server start    
* After cluster installation    
* Before cluster termination   

You can upload your recipes to Cloudbreak via the UI or CLI. Then, when creating a cluster, you can optionally attach one or more "recipes" and they will be executed on a specific host group at a specified time. 


### Writing recipes

When using recipes, consider the following:

* The scripts will be executed on the node types you specify (such as "master", "worker", "compute"). If you want to run a a script on all nodes, define the recipe one per node type.  
* The script will execute on all of the nodes of that type as root.  
* In order to be executed, your script must be in a network location which is accessible from the cloud controller and cluster instances VPC.  
* Make sure to follow Linux best practices when creating your scripts. For example, don't forget to script "Yes" auto-answers where needed.  
* Do not execute yum update –y since it may update other components on the instances (such as salt), which can create unintended or unstable behavior.   
* The scripts will be executed as root. The recipe output is written to `/var/log/recipes` on each node on which it was executed.
 

#### Sample recipe for yum proxy setting

```
#!/bin/bash
cat >> /etc/yum.conf <<ENDOF
proxy=http://10.0.0.133:3128
ENDOF
```


### Add recipes

In order to use your recipe for clusters, you must register it first by using the steps below.

**Steps**

1. Place your script in a network location accessible from Cloudbreak and cluster instances virtual network. 

2. Select **External Sources > Recipes** from the navigation menu. 

3. Click on **Create Recipe**. 

4. Provide the following:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your recipe. |
| Description | (Optional) Enter a description for your recipe.|
| Execution Type | Select one of the following options: <ul><li>**pre-ambari-start**: The script will be executed prior to Ambari server start.</li><li>**post-ambari-start**: The script will be executed after Ambari server start but prior to cluster installation.</li><li>**post-cluster-install**: The script will be executed after cluster deployment.</li><li>**pre-termination**: The script will be executed before cluster termination.</li></ul>|
| Script | <p>Select one of: <ul><li>**Script**: Paste the script.</li><li> **File**: Point to a file on your machine that contains the recipe.</li><li> **URL**: Specify the URL for your recipe.</li></ul> |
    
5. When creating a cluster, you can select previously added recipes on the advanced **Cluster Extensions** page of the cluster wizard. 



