
Ext JS is GPL licensed software and is no longer included in builds of HDP 2.6. Because of this, the Oozie WAR file is not built to include the Ext JS-based user interface unless Ext JS is manually installed on the Oozie server. If you add Oozie using Ambari 2.6.1.0 to an HDP 2.6.4 or greater stack, no Oozie UI will be available by default. Therefore, if you plan to use Oozie web UI with Ambari 2.6.1.0 and HDP 2.6.4 or greater, you you must manually install Ext JS on the Oozie server host.

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