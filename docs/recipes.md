## Recipes

Although Cloudbreak lets you provision HDP clusters in the cloud based on custom Ambari blueprints, Cloudbreak provisioning options don't consider all possible use cases. For that reason, we introduced recipes. 

A recipe is a script that runs on all nodes of a selected node group before or after the Ambari cluster installation. You can use recipes for tasks such as installing additional software or performing advanced cluster configuration. For example, you can use a recipe to put a JAR file on the Hadoop classpath.

When creating a cluster, you can optionally upload one or more "recipes" (custom scripts) and they will be executed on a specific host group at a specified time. Available recipe execution times are:  

* Before Ambari server start    
* After Ambari server start    
* After cluster installation    
* Before cluster termination   


### Writing Recipes

When using recipes, consider the following:

* The scripts will be executed on the node types you specify (such as "master", "worker", "compute"). If you want to run a a script on all nodes, define the recipe one per node type.  
* The script will execute on all of the nodes of that type as root.  
* In order to be executed, your script must be in a network location which is accessible from the cloud controller and cluster instances VPC.  
* Make sure to follow Linux best practices when creating your scripts. For example, don't forget to script "Yes" auto-answers where needed.  
* Do not execute yum update –y since it may update other components on the instances (such as salt), which can create unintended or unstable behavior.   
* The scripts will be executed as root. The recipe output is written to `/var/log/recipes` on each node on which it was executed.
 

#### Sample Recipe for Yum Proxy Setting

```
#!/bin/bash
cat >> /etc/yum.conf <<ENDOF
proxy=http://10.0.0.133:3128
ENDOF
```

#### Recipe to Install Ext JS for Oozie 

Ext JS is GPL licensed software and is no longer included in builds of HDP 2.6. Because of this, the Oozie WAR file is not built to include the Ext JS-based user interface unless Ext JS is manually installed on the Oozie server. If you add Oozie using Ambari 2.6.1.0 to an HDP 2.6.4 or greater stack, no Oozie UI will be available by default. Therefore, if you plan to use Oozie with Ambari 2.6.1.0 to an HDP 2.6.4 or greater, you must ensure that ExtJS is installed prior to installing Oozie (that is, prior to cluster install). 

You can install Ext JS by adding the following PRE-AMBARI-START recipe:

<pre>export EXT_JS_VERSION=2.2-1
     export OS_NAME=centos6
     wget http://public-repo-1.hortonworks.com/HDP-UTILS-GPL-1.1.0.22/repos/$OS_NAME/extjs/extjs-$EXT_JS_VERSION.noarch.rpm
     rpm -ivh extjs-$EXT_JS_VERSION.noarch.rpm</pre> 
     
Make the following changes to the script:

* Change the EXT_JS_VERSION to the specific ExtJS version that you want to use.  
* Change the OS_NAME to the name of the operating system. Supported values are: centos6, centos7, centos7-ppc.

The general steps are:

1. Be sure to review and agree to the Ext JS license prior to using this recipe.  
2. Create a PRE-AMBARI-START recipe. For instructions on how to create a recipe, refer to [Add Recipes](#add-recipes).   
3. When creating a cluster, choose this recipe to be executed on all host groups of the cluster.  



### Add Recipes

To add a recipe, perform these steps.

**Steps**

1. Place your script in a network location accessible from Cloudbreak and cluster instances virtual network. 

2. Select **Blueprints** from the navigation pane. 

3. Click on **Create Blueprint**. 

4. Provide the following:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your recipe. |
| Description | (Optional) Enter a description for your recipe.|
| Execution Type | Select one of the following options: <ul><li>**pre-ambari-start**: The script will be executed prior to Ambari server start.</li><li>**post-ambari-start**: The script will be executed after Ambari server start but prior to cluster installation.</li><li>**post-cluster-install**: The script will be executed after cluster deployment.</li><li>**pre-termination**: The script will be executed before cluster termination.</li></ul>|
| Script | <p>Select one of: <ul><li>**Script**: Paste the script.</li><li> **File**: Point to a file on your machine that contains the recipe.</li><li> **URL**: Specify the URL for your recipe.</li></ul> |

    <a href="../images/cb-recipe-add.png" target="_blank" title="click to enlarge"><img src="../images/cb-recipe-add.png" width="650" title="Cloudbreak web UI"></a> 
    
3. When creating a cluster, you can select previously added recipes in the **Recipes** section. 

    <a href="../images/cb-recipe-select.png" target="_blank" title="click to enlarge"><img src="../images/cb-recipe-select.png" width="650" title="Cloudbreak web UI"></a> 



### Delete Recipes

You can delete previously added items by selecting and item and using the **delete** option. 

