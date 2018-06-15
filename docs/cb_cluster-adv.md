# Configuring advanced cluster options 

## Configuring the gateway

When creating a cluster, Cloudbreak installs and configures a gateway, powered by [Apache Knox](https://knox.apache.org/), to protect access to the cluster resources:

* This gateway is installed on the same host as the Ambari server.  
* By default, transport layer security on the gateway endpoint is via a *self-signed SSL certificate* on port 8443. This is illustrated on the following diagram:   
    
    <img src="../images/cb_gateway.png" width="650" title="Cluster Information"> 

* The cluster resources that are accessible through the gateway are determined by the settings provided on the **Gateway Configuration** page of the basic create cluster wizard.  
* By default, the gateway is deployed and *Ambari* is proxied through the gateway.  
* The choice of cluster services to expose and proxy through the gateway depends on your blueprint. Cloudbreak analyzes your blueprint and provides a list of services that can be exposed through the gateway. You should review this list and select the services that should be proxied through the gateway.  
* If you do not enable the gateway, or you do not expose Ambari (or any other service) through the gateway, you must configure access to those services on the security group on your own. 


### Services available via gateway 

The following cluster resources are available for access via the gateway endpoint:

| Cluster resource | URL | 
|---|---|
| Ambari | https://{gateway-host}:8443/{cluster-name}/{topology-name}/ambari/ |
| Hive and Hive Server Interactive<sup>*</sup> | https://{gateway-host}:8443/{cluster-name}/{topology-name}/hive/ |
| Job History Server | https://{gateway-host}:8443/{cluster-name}/{topology-name}/jobhistory/ |
| Name Node | https://{gateway-host}:8443/{cluster-name}/{topology-name}/hdfs/ |
| WebHDFS | https://{gateway-host}:8443/{cluster-name}/{topology-name}/webhdfs/ |
| Resource Manager | https://{gateway-host}:8443/{cluster-name}/{topology-name}/yarn/ |
| Spark History Server | https://{gateway-host}:8443/{cluster-name}/{topology-name}/sparkhistory/ |
| Zeppelin | https://{gateway-host}:8443/{cluster-name}/{topology-name}/zeppelin/ |

<sup>*</sup> Refer to [Accessing Hive via JDBC](https://hortonworks.github.io/cloudbreak-documentation/latest/) for more information.

The following applies:

* The `gateway-host` is the IP address or hostname of the Ambari server node.  
* The `cluster-name` is the name of the cluster.  
* The `topology-name` is the name of the gateway topology that you entered when creating the cluster. By default this is set to `db-proxy`.    


### Configure the gateway 

When creating a cluster, you can configure the gateway on the **Gateway Configuration** page of the basic create cluster wizard. 

**Steps**

1. In the create cluster wizard, navigate to the **Gateway Configuration** page: 

    <img src="../images/cb_cb-gateway01.png" width="650" title="Cluster Information"> 
    
    
2. Under **Gateway Topology**:

    * The gateway option is enabled by default.  
    * The **Name** for your gateway is set to `db-proxy` by default. You can update it if you would like. This name is used in the URLs for the cluster resources, as described in [Services available via gateway](#services-available-via-gateway). 
    * Under **Exposable Services**, the choice of cluster services to expose and proxy through the gateway depends on your blueprint. Cloudbreak analyzes your blueprint and provides a list of services that can be exposed through the gateway. You should review this list and select the services that should be proxied through the gateway. By default, only Ambari is exposed through the gateway. 
    * We recommend that you expose the following services through the gateway:
        * **Analytics blueprint**: Hive and Zeppelin
        * **ETL blueprint**: No additional services need to be exposed
        * **Data science**: Spark and Zeppelin   

3. Under **Exposable Services**, use the dropdown to select services that should be exposed via the gateway. To expose a service, select it and click **Expose**. Select **ALL** to expose all. 


### Obtain URLs for cluster resources 

Once your cluster is running, you can obtain Ambari URL and the URLs of the cluster services from the **Gateway** tab in the cluster details:

> This tab is only available when gateway is enabled. 

<img src="../images/cb_cb-gateway02.png" width="650" title="Cluster Information">

The URL structure is as described in [Services available via gateway](#services-available-via-gateway).


### Configure single sign-on (SSO) 

When creating a cluster, if you selected to configure a gateway, on the advanced **Gateway Configuration** page of the advanced create cluster wizard, you can also configure the gateway to be the SSO identity provider. 

> This option is technical preview. 

**Prerequisites**

You must have an existing authentication source (LDAP or AD) and register it with Cloudbreak, as described in [Using an external authentication source for clusters](https://hortonworks.github.io/cloudbreak-documentation/latest/). 

**Steps**

1. In the create cluster wizard, select the advanced mode.  
1. On the **External Sources** page, under **Configure Authentication**, select to attach a previously configured LDAP to the cluster.  
2. On the **Gateway Configuration** page, under **Single Sign On (SSO)**, click the toggle button to enable SSO.


## Tagging resources 

When you manually create resources in the cloud, you have an option to add custom tags that help you track these resources. Likewise, when creating clusters, you can instruct Cloudbreak to tag the cloud resources that it creates on your behalf. The tags added during cluster creation are displayed in your cloud account on the resources that Cloudbreak provisioned for your clusters. 

You can use tags to categorize your cloud resources by purpose, owner, and so on. Tags come in especially handy when you are using a corporate AWS account and you want to quickly identify which resources belong to your cluster(s). In fact, your corporate cloud account admin may require you to tag all the resources that you create, in particular resources, such as VMs, which incur charges.


### Add tags when creating a cluster

You can tag the cloud resources used for a cluster by providing custom tag names and values when creating a cluster via UI or CLI. In the Cloudbreak UI, this option is available in the create cluster wizard, in the advanced **General Configuration** page > **Tags** section. 

It is not possible to add tags via Cloudbreak after your cluster has been created.  

[comment]: <> (Commenting out the content which does not apply but we may want to add it in the future.)
[comment]: <> (When you clone your cluster, all tags associated with the source cluster will be added to the template of the clone.)  
[comment]: <> (When you save a cluster template, all tags will be saved as part of the template, and they will be listed on the cluster template page.)    


To learn more about tags and their restrictions, refer to the cloud provider documentation: 
 
* [Tags on AWS](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)    
* [Tags on Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags)  
* [Labels on GCP](https://cloud.google.com/resource-manager/docs/using-labels)  
* [Tags on OpenStack](https://docs.openstack.org/mitaka/networking-guide/ops-resource-tags.html)  


### Add tags in Profile (AWS)

In order to differentiate launched instances, you can optionally define custom tags for your AWS resources deployed by Cloudbreak. 

* If you want just one custom tag for your CloudFormation resources, set this variable in the `Profile`:

    ```export CB_AWS_DEFAULT_CF_TAG=mytagcontent```

    In this example, the name of the tag will be `CloudbreakId` and the value will be `mytagcontent`.

* If you prefer to customize the tag name, set this variable:

    ```export CB_AWS_CUSTOM_CF_TAGS=mytagname:mytagvalue```

    In this example the name of the tag will be `mytagname` and the value will be `mytagvalue`. 

* You can specify a list of tags with a comma separated list: 

    ```export CB_AWS_CUSTOM_CF_TAGS=tag1:value1,tag2:value2,tag3:value3```
    
    


## Configuring autoscaling 

Autoscaling allows you to adjust cluster capacity based on Ambari metrics and alerts, as well as schedule time-based capacity adjustment. When creating an autoscaling policy, you define:

* An **alert** that triggers a scaling policy. An alert can be based on an Ambari metric or can be time-based.     
* A **scaling policy** that adds or removes a set number of nodes to a selected host group when the conditions defined in the attached alert are met.    

**Metric-based autoscaling**

Cloudbreak accesses all available Ambari metrics and allows you to define alerts based on these metrics. For example:

| Alert Definition | Policy Definition |
|---|---|
| *ResourceManager CPU* alert with *CRITICAL* status for 5 minutes | Add 10 worker nodes |
| *HDFS Capacity Utilization* alert with *WARN* status for 20 minutes | Set the number of worker nodes to 50 |
| *Ambari Server Alerts* alert with *CRITICAL* status for 15 minutes | Decrease the number of worker nodes by 80% |


**Time-based autoscaling**

Time-based alerts can be defined by providing a cron expression. For example: 

| Alert Definition | Policy Definition |
|---|---|
| Every day at 07:00 AM (GMT-8) | Add 90 worker nodes | 
| Every day at 08:00 PM (GMT-8) | Remove 90 worker nodes |

> Cluster resizing is not supported for HDF clusters. 
 

### Enable autoscaling 

For each newly created cluster, autoscaling is disabled by default but it can be enabled once the cluster is in a running state. 

> Autoscaling configuration is only available in the UI. It is currently not available in the CLI. 

**Steps**

1. On the cluster details page, navigate to the **Autoscaling** tab.   
3. Click the toggle button to enable autoscaling:

    <img src="../images/cb_cb-autoscaling1.png" width="650" title="Autoscaling in Cloudbreak UI"> 
      
4. The toggle button turns green and you can see that "Autoscaling is enabled".   
5. [Define alerts](#defining-an-alert) and then [define scaling policies](#create-a-scaling-policy). You can also [adjust the autoscaling settings](#configure-autoscaling-settings). 

If you decide to disable autoscaling, your previously defined alerts and policies will be preserved. 


### Defining an alert

After you have enabled autoscaling, define a metric-based or time-based alert.  


#### Define a metric-based alert 

After [enabling autoscaling](#enable-autoscaling), perform the following steps to create a metric-based alert.  

> If you would like to change default thresholds for an Ambari metric, refer to [Modifying alerts](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-operations/content/modifying_alerts.html) in Ambari documentation. 

> If you would like to create a custom Ambari alert, refer to [How to create a custom Ambari alert and use it for Cloudbreak autoscaling policies](https://community.hortonworks.com/articles/143762/how-to-create-a-custom-ambari-alert-and-use-it-for.html).

**Steps**

1. In the **Alert Configuration** section, select **Metric Based** alert type.      
2. Provide the following information:

    | Parameter | Description |
|---|---|
| Enter alert name | Enter a unique name for the alert. | 
| Choose metric type | Select the Ambari metric that should trigger the alert. |
| Alert status | Select the alert status that should trigger an alert for the selected metric. One of: OK, CRITICAL, WARNING. | 
| Alert duration | Select the alert duration that should trigger an alert. |   

3. Click **+** to save the alert.  

Once you have defined an alert, [create a scaling policy](#create-a-scaling-policy) that this metric should trigger.




#### Define a time-based alert 

After [enabling autoscaling](#enable-autoscaling), perform the following steps to create a time-based alert.

**Steps**
 
1. In the **Alert Configuration** section, select the **Time Based** alert type. 
2. Provide the following information: 

    | Parameter | Description |
|---|---|
| Enter alert name. |  Enter a unique name for the alert. | 
| Select timezone. | Select your timezone. |   
| Enter cron expression | Enter a cron expression that defines the frequency of the alert. Refer to [Cron expression generator](http://www.cronmaker.com/). | 

3. Click **+** to save the alert.   

Once you have defined an alert, [create a scaling policy](#create-a-scaling-policy) that this metric should trigger.


### Create a scaling policy 

After [enabling autoscaling](#enable-autoscaling) and [creating at least one alert](#defining-an-alert), perform the following steps to create a scaling policy.

**Steps**

1. In the **Policy Configuration** section, provide the following information:

    | Parameter | Description |
|---|---| 
| Enter policy name | Enter a unique name for the policy. | 
| Select action | Select one of the following actions: Add (to add nodes to a host group) Remove (to delete nodes from a host group), or Set (to set the number of nodes in a host group to the chosen number). | 
| Enter number or percentage | Enter a number defining how many or what percentage of nodes to add or remove. If the action selected is "set", this defines the number of nodes that a host group will be set to after scaling. |  
| Select nodes of percent | Select "nodes" or "percent", depending on whether you want to scale to a specific number, or percent of current number of nodes.  |
| Select host group | Select the host group to which to apply the scaling. | 
| Choose an alert | Select the alert based on which the scaling should be applied. |   

9. Click **+** to save the alert.   



### Configure autoscaling settings 

After [enabling autoscaling](#enable-autoscaling), perform these steps to configure the auto scaling settings for your cluster.   

**Steps**

1. In the **Cluster Scaling Configuration**, provide the following information: 
    
    | Setting | Description	 | Default Value |
|---|---|---|
| Cooldown time  | After an auto scaling event occurs, the amount of time to wait before enforcing another scaling policy. This means that the scaling events scheduled during cooldown time are dropped. | 30 minutes |
| Minimum Cluster Size |	The minimum size allowed for the cluster. Auto scaling policies cannot scale the cluster below or above this size. | 2 nodes |
| Maximum Cluster Size |	The maximum size allowed for the cluster. Auto scaling policies cannot scale the cluster below or above this size. | 100 nodes |

2. Click **Save** to save the changes. 



## Using custom blueprints


This option allows you to create and save your custom blueprints. 

**Ambari blueprints** are your declarative definition of your HDP or HDF cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. 

You have two options concerning using blueprints with Cloudbreak:

* **Use one of the pre-defined blueprints**: To use one of the default blueprints, simply select them when creating a cluster. The option is available on the **General Configuration** page. First select the **Stack Version** and then select your chosen blueprint under **Cluster Type**. For the list of default blueprints, refer to [Default cluster configurations](https://hortonworks.github.io/cloudbreak-documentation/latest/).       
* **Add your custom blueprint** by uploading a JSON file or pasting the JSON text. 

We recommend that you review the default blueprints to check if they meet your requirements. You can do this by selecting **Blueprints** from the navigation pane in the Cloudbreak web UI or by reading the documentation below.
 

### Creating blueprints 

Ambari blueprints are specified in JSON format. A blueprint can be exported from a running Ambari cluster and can be reused in Cloudbreak after slight modifications. When a blueprint is exported, it includes  some hardcoded configurations such as domain names, memory configurations, and so on, that are not applicable to the Cloudbreak cluster. There is no automatic way to modify an exported blueprint and make it instantly usable in Cloudbreak, the modifications have to be done manually. 

In general, the blueprint should include the following elements:

<pre>"Blueprints": {
    "blueprint_name": "hdp-small-default",
    "stack_name": "HDP",
    "stack_version": "2.6"
  },
  "settings": [],
  "configurations": [],
  "host_groups": [
  {
      "name": "master",
      "configurations": [],
      "components": []
    },
    {
      "name": "worker",
      "configurations": [],
      "components": [ ]
    },
    {
      "name": "compute",
      "configurations": [],
      "components": []
    }
   ]
  }</pre>
  
For correct blueprint layout and other information about Ambari blueprints, refer to the [Ambari cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints) page. 

Ambari 2.6.1 or newer does not install the mysqlconnector; therefore, when creating a blueprint for Ambari 2.6.1 or newer <b>you should not include the MYSQL_SERVER component</b> for Hive Metastore in your blueprint. Instead, you have two options:
<ul>
<li>Configure an external RDBMS instance for Hive Metastore and include the JDBC connection information in your blueprint. If you choose to use an external database that is not PostgreSQL (such as Oracle, mysql) you must also set up Ambari with the appropriate connector; to do this, create a pre-ambari-start recipe and pass it when creating a cluster.</li>
<li>If a remote Hive RDBMS is not provided, Cloudbreak installs a Postgres instance and configures it for Hive Metastore during the cluster launch.</li>
</ul>
For information on how to configure an external database and pass your external database connection parameters, refer to <a href="https://cwiki.apache.org/confluence/display/AMBARI/Blueprints">Ambari blueprint</a> documentation.

Cloudbreak requires you to define an additional element in the blueprint called "blueprint_name". This should be a unique name within Cloudbreak list of blueprints. For example:

<pre>"Blueprints": {
    "blueprint_name": "hdp-small-default",
    "stack_name": "HDP",
    "stack_version": "2.6"
  },
  "settings": [],
  "configurations": [],
  "host_groups": [
  ...
</pre>

The "blueprint_name" is not included in the Ambari export. 

After you provide the blueprint to Cloudbreak, the host groups in the JSON will be mapped to a set of instances when starting the cluster, and the specified services and components will be installed on the corresponding nodes. It is not necessary to define a complete configuration in the blueprint. If a configuration is missing, Ambari will use a default value. 

Here are a few [blueprint examples](https://github.com/hortonworks/cloudbreak/tree/master/core/src/main/resources/defaults/blueprints). You can also refer to the default blueprints provided in the Cloudbreak UI.


[comment]: <> (TO-DO: Maybe we can find some newer examples?)


### Creating dynamic blueprints    

Cloudbreak allows you to create [dynamic blueprints](https://hortonworks.github.io/cloudbreak-documentation/latest/), which include templating: the values of the variables specified in the blueprint are dynamically replaced in the cluster creation phase, picking up the parameter values that you provided in the Cloudbreak UI or CLI.
Cloudbreak supports mustache kind of templating with {{{variable}}} syntax. 

> You cannot use functions in the blueprint file; only variable injection is supported.

#### External authentication source (LDAP/AD)

When using [external authentication sources](https://hortonworks.github.io/cloudbreak-documentation/latest/), the following variables can be specified in your blueprint for replacement:

| Variable | Description | Example |
|---|---|---|
| ldap.connectionURL | the URL of the LDAP (host:port) | ldap://10.1.1.1:389 |
| ldap.bindDn | The root Distinguished Name to search in the directory for users | CN=Administrator,CN=Users,DC=ad,DC=hdc,DC=com |
| ldap.bindPassword | The root Distinguished Name password | Password1234! |
| ldap.directoryType | The directory of type | LDAP or ACTIVE_DIRECTORY |
| ldap.userSearchBase | User search base | CN=Users,DC=ad,DC=hdc,DC=com |
| ldap.userNameAttribute | Username attribute | cn |
| ldap.userObjectClass | Object class for users | person |
| ldap.groupSearchBase | Group search base | OU=Groups,DC=ad,DC=hdc,DC=com|
| ldap.groupNameAttribute | Group attribute | cb |
| ldap.groupObjectClass | Group object class | group |
| ldap.groupMemberAttribute | Attribute for membership | member |
| ldap.domain | Your domain | example.com |

#### External database (RDBMS)

When using [external databases](https://hortonworks.github.io/cloudbreak-documentation/latest/), the following variables can be specified in your blueprint for replacement:

| Variable | Description | Example |
|---|---|---|
| rds.[type].connectionString | The jdbc url to the RDBMS | jdbc:postgresql://db.test:5432/test |
| rds.[type].connectionDriver | The connection driver | org.postgresql.Driver |
| rds.[type].connectionUserName | The user name to the database | admin |
| rds.[type].connectionPassword | The password for the connection | Password1234! |  
| rds.[type].subprotocol | Parsed from jdbc url | postgres  |
| rds.[type].databaseEngine | Capital database name | POSTGRES |


### Upload blueprints  

Once you have your blueprint ready, perform these steps.

**Steps**

1. In the Cloudbreak UI, select **Blueprints** from the navigation pane. 
2. To add your own blueprint, click **Create Blueprint** and enter the following parameters:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your blueprint. |
| Description | (Optional) Enter a description for your blueprint.|
| Blueprint Source | <p>Select one of: <ul><li>**Text**: Paste blueprint in JSON format.</li><li> **File**: Upload a file that contains the blueprint.</li><li> **URL**: Specify the URL for your blueprint.</li></ul> |

    <img src="../images/cb_cb-blueprint-add.png" width="650" title="Cloudbreak web UI">

2. To use the uploaded blueprints, select it when creating a cluster. The option is available on the **General Configuration** page. First select the **Platform Version** and then select your chosen blueprint under **Cluster Type**. 

    <img src="../images/cb_cb-blueprint-select.png" width="650" title="Cloudbreak web UI">


## Set custom properties

Cloudbreak allows you to create dynamic blueprints with property variables and set properties on a per-cluster basis by providing property values during cluster creation. 

In order to set custom properties for a cluster you must:

1. Create a blueprint that includes property variables for the properties that you want to set.  
2. When creating a cluster, select the blueprint and then specify the property values under **Cluster Extensions** > **Custom Properties** in the advanced view of the cluster wizard.     

In the cluster creation phase, the property values in the blueprint will be replaced based on the input, picking up the parameter values that you provided.

**Steps**

1. Prepare a blueprint which includes a template for the properties that you would like to set. Make sure to:

    * Include these templates in the “configurations” section of your blueprint.  
    * Use the mustache format. Cloudbreak supports [mustache](https://mustache.github.io/) kind of templating with {{{variable}}} syntax, so your templates must be in the mustache format.   

    **Example:**      
    This example provides a template for setting three properties:

    * `fs.trash.interval`  
    * `hadoop.tmp.dir`  
    * `hive.exec.compress.output`    

    <pre>...
    {
      "core-site": {
        "fs.trash.interval": "{{{ fs.trash.interval }}}",
        "hadoop.tmp.dir": "{{{ my.tmp.directory }}}"
      }
    },
    {
      "hive-site": {
        "hive.exec.compress.output": "{{{ hive.exec.compress.output }}}"
      }
    },
...</pre>


2. When creating a cluster:

    1. Under **General Configuration > Cluster Type**, select the blueprint prepared in the previous step.  
    2. In the advanced view of the cluster wizard, under **Cluster Extensions > Custom Properties**, include a JSON file which defines the property values.

        **Example:**  
        The following JSON entry sets the values for the properties from the previous step: 

        <pre>{
    "fs.trash.interval": "4320",
    "hive.exec.compress.output": "true",
    "my.tmp.directory": "/hadoop/tmp"
}</pre>


3. As a result, the values of `hive.exec.compress.output`, `my.tmp.directory` and `fs.trash.interval` will be replaced in the blueprint based on the input that you provided. 

    **Example:**  
    The property values will be replaced for the cluster as follows based on what was defined in the previous step: 

    <pre>...
   {
     "core-site": {
       "fs.trash.interval": "4320",
       "hadoop.tmp.dir": "/hadoop/tmp"
     }
   },
   {
     "hive-site": {
       "hive.exec.compress.output": "true"
     }
   },</pre>




## Using custom images

Default images are available for each supported cloud provider and region. The following table lists the default base images available:

[Comment]: <> (For AWS and Azure, per region images are provided.)

| Cloud provider | Default image |
|---|---|
| AWS | Amazon Linux 2017 |
| Azure | CentOS 7 |
| GCP | CentOS 7 |  
| OpenStack | CentOS 7 |

Since these default images may not fit the requirements of some users (for example when user requirements include custom OS hardening, custom libraries, custom tooling, and so on) Cloudbreak allows you to use your own **custom base images**.

In order to use your own custom base images you must:

1. Build your custom images  
2. Prepare the custom image catalog JSON file and save it in a location accessible to the Cloudbreak VM  
3. Register your custom image catalog with Cloudbreak    
4. Select a custom image when creating a cluster  

> Only <strong>base images</strong> can be created and registered as custom images. Do not create or register <strong>prewarmed images</strong> as custom images.



### Build custom images

Refer to [Custom images for Cloudbreak](https://github.com/hortonworks/cloudbreak-images) for information on how to build custom images.

This repository includes instructions and scripts to help you build custom images. Once you have the images, refer to the documentation below for information on how to create an image catalog and register it with Cloudbreak.


### Prepare the image catalog

Once you've built the custom images, prepare your custom image catalog JSON file. Once your image catalog JSON file is ready, save it in a location accessible via HTTP/HTTPS.


#### Structure of the image catalog JSON file

The image catalog JSON file includes the following two high-level sections:

* `images`: Contains information about the created images. The burned images are stored in the `base-images` section.  
* `versions`: Contains the `cloudbreak` entry, which includes mapping between Cloudbreak versions and the image identifiers of burned images available for these Cloudbreak versions.

> After adding your image(s) to the `images` section, make sure to also update the `versions` section. 

**Images section**  

The burned images are stored in the `base-images` sub-section of `images`. The `base-images` section stores one or more image "records". Every image "record" must contain the date, description, images, os, os_type, and uuid fields.

| Parameter | Description |
|---|---|
| date | Date for your image catalog entry. |
| description | Description for your image catalog entry. |
| images | The image sets by cloud provider. An image set must store the virtual machine image IDs by the related region of the provider (AWS, Azure) or contain one default image for all regions (GCP, OpenStack). The virtual machine image IDs come from the result of the image burning process and must be an existing identifier of a virtual machine image on the related provider side. For the providers which use global rather than per-region images, the region should be replaced with **`default`**. |
| os | The operating system used in the image. |
| os_type | The type of operating system which will be used to determine the default Ambari and HDP/HDF repositories to use. Set `os_type` to "redhat6" for amazonlinux or centos6 images. Set `os_type` to "redhat7" for centos7 or rhel7 images. |
| uuid | The `uuid` field must be a unique identifier within the file. You can generate it or select it manually. The utility `uuidgen` available from your command line is a convenient way to generate a unique ID. |

**Versions section**  

The `versions` section includes a single "cloudbreak" entry, which maps the uuids to a specific Cloudbreak version:

| Parameter | Description |
|---|---|
| images | Image `uuid`, same as the one that you specified in the `base-images` section. |
| versions | The Cloudbreak version(s) for which you would like to use the images. |


#### Example image catalog JSON file

Here is an example image catalog JSON file that includes two sets of custom base images:

* A custom base image for AWS:
    * That is using Amazon Linux operating system
    * That will use the Redhat 6 repos as default Ambari and HDP repositories during cluster create     
    * Has a unique ID of "44b140a4-bd0b-457d-b174-e988bee3ca47"
    * Is available for Cloudbreak 2.4.0    
*  A custom base image for Azure, Google, and OpenStack:
    * That is using CentOS 7 operating system
    * That will use the Redhat 7 repos as default Ambari and HDP repositories during cluster create   
    * Has a unique ID of "f6e778fc-7f17-4535-9021-515351df3692"
    * Is available to Cloudbreak 2.4.0      


You can also download it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/custom-image-catalog.json).


<pre><small>
{
  "images": {
    "base-images": [
      {
        "date": "2017-10-13",
        "description": "Cloudbreak official base image",
        "images": {
          "aws": {
            "ap-northeast-1": "ami-78e9311e",
            "ap-northeast-2": "ami-84b613ea",
            "ap-southeast-1": "ami-75226716",
            "ap-southeast-2": "ami-92ce23f0",
            "eu-central-1": "ami-d95be5b6",
            "eu-west-1": "ami-46429e3f",
            "sa-east-1": "ami-86d5abea",
            "us-east-1": "ami-51a2742b",
            "us-west-1": "ami-21ccfe41",
            "us-west-2": "ami-2a1cdc52"
          }
        },
        "os": "amazonlinux",
        "os_type": "redhat6",
        "uuid": "44b140a4-bd0b-457d-b174-e988bee3ca47"
      },
      {
        "date": "2017-10-13",
        "description": "Cloudbreak official base image",
        "images": {
          "azure": {
            "Australia East": "https://hwxaustraliaeast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Australia South East": "https://hwxaustralisoutheast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Brazil South": "https://sequenceiqbrazilsouth2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Canada Central": "https://sequenceiqcanadacentral.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Canada East": "https://sequenceiqcanadaeast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Central India": "https://hwxcentralindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Central US": "https://sequenceiqcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East Asia": "https://sequenceiqeastasia2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East US": "https://sequenceiqeastus12.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East US 2": "https://sequenceiqeastus22.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Japan East": "https://sequenceiqjapaneast2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Japan West": "https://sequenceiqjapanwest2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Korea Central": "https://hwxkoreacentral.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Korea South": "https://hwxkoreasouth.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "North Central US": "https://sequenceiqorthcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "North Europe": "https://sequenceiqnortheurope2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "South Central US": "https://sequenceiqouthcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "South India": "https://hwxsouthindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Southeast Asia": "https://sequenceiqsoutheastasia2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "UK South": "https://hwxsouthuk.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "UK West": "https://hwxwestuk.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West Central US": "https://hwxwestcentralus.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West Europe": "https://sequenceiqwesteurope2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West India": "https://hwxwestindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West US": "https://sequenceiqwestus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West US 2": "https://hwxwestus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd"
          },
          "gcp": {
            "default": "sequenceiqimage/hdc-hdp--1710161226.tar.gz"
          },
          "openstack": {
            "default": "hdc-hdp--1710161226"
          }
        },
        "os": "centos7",
        "os_type": "redhat7",
        "uuid": "f6e778fc-7f17-4535-9021-515351df3691"
      }
    ]
},
  "versions": {
    "cloudbreak": [
      {
        "images": [
          "44b140a4-bd0b-457d-b174-e988bee3ca47",
          "f6e778fc-7f17-4535-9021-515351df3692"
        ],
        "versions": [
          "2.4.0"
        ]
      }
    ]
  }
}
</small></pre>


### Register image catalog

Now that you have created your image catalog JSON file, register it with your Cloudbreak instance. You can do this via Cloudbreak UI, CLI, or be editing the Profile file. 

> The content type of your image catalog file should be <i>"application/json"</i> for Cloudbreak to be able to process it.


#### Register image catalog in the UI

Use these steps to register your custom image catalog in the Cloudbreak UI.

**Steps**

1. In the Cloudbreak UI, select **External Sources** > **Image Catalogs** from the navigation menu.  
2. Click **Create Image Catalog**.  
3. Enter name for your image catalog and the URL to the location where it is stored.  
4. Click **Create**.

After performing these steps, the image catalog will be available and automatically selected as the default entry in the image catalog drop-down list in the create cluster wizard.

#### Register image catalog in the CLI

To register your custom image catalog using the CLI, use the `cb imagecatalog create` command. Refer to [CLI documentation](https://hortonworks.github.io/cloudbreak-documentation/latest/).

#### Register image catalog in the Profile 

As an alternative to using the UI or CLI, it is possible to place the catalog file to the Cloudbreak deployer`s etc directory and then set CB_IMAGE_CATALOG_URL variable in your Profile to IMAGE_CATALOG_FILE_NAME.JSON. 

**Steps**

1. On the Cloudbreak machine, switch to the root user by using `sudo su`  
2. Save the image catalog file on your Cloudbreak machine in the /var/lib/cloudbreak-deployment/etc directory.  
3. Edit the Profile file located in /var/lib/cloudbreak-deployment by adding export CB_IMAGE_CATALOG_URL to the file and set it to the name of your JSON file which declares your custom images. For example: `export CB_IMAGE_CATALOG_URL=custom-image-catalog.json`    
4. Save the Profile file.  
5. Restart Cloudbreak by using `cbd restart`.  


### Select a custom image when creating a cluster

Once you have registered your image catalog, you can use your custom image(s) when creating a cluster.

#### Select a custom image in Cloudbreak web UI

Perform these steps in the advanced **General Configuration** section of the create wizard wizard.

**Steps**  

1. In the create cluster wizard, make sure that you are using the advanced wizard version. You need to perform the steps in the **Image Settings** section.  
1. Under **Choose Image Catalog**, select your custom image catalog.  
2. Under **Image Type**, select "Base Image".
2. Under **Choose Image**, select the provider-specific image that you would like to use.   
    The "os" that you specified in the image catalog will be displayed in the selection and the content of the "description" will be displayed in green.    
3. You can leave the default entries for the Ambari and HDP/HDF repositories, or you can customize to point to specific versions of Ambari and HDP/HDF that you want to use for the cluster.  

    <img src="../images/cb_cb-images.png" width="650" title="Cloudbreak UI">


#### Select a custom image in the CLI

To use the custom image when creating a cluster via CLI, perform these steps.  

**Steps**  

1. Obtain the image ID. For example:

    <pre>cb imagecatalog images aws --imagecatalog custom-catalog
[
  {
    "Date": "2017-10-13",
    "Description": "Cloudbreak official base image",
    "Version": "2.5.1.0",
    "ImageID": "44b140a4-bd0b-457d-b174-e988bee3ca47"
  },
  {
    "Date": "2017-11-16",
    "Description": "Official Cloudbreak image",
    "Version": "2.5.1.0",
    "ImageID": "3c7598a4-ebd6-4a02-5638-882f5c7f7add"
  }
]</pre>

2. When preparing a CLI JSON template for your cluster, set the "ImageCatalog" parameter to the image catalog that you would like to use, and set the "ImageId" parameter to the uuid of the image from that catalog that you would like to use. For example:

    <pre>...
  "name": "aszegedi-cli-ci",
  "network": {
    "subnetCIDR": "10.0.0.0/16"
  },
  "orchestrator": {
    "type": "SALT"
  },
  "parameters": {
    "instanceProfileStrategy": "CREATE"
  },
  "region": "eu-west-1",
  "stackAuthentication": {
    "publicKeyId": "seq-master"
  },
  "userDefinedTags": {
    "owner": "aszegedi"
  },
  "imageCatalog": "custom-catalog",
  "imageId": "3c7598a4-ebd6-4a02-5638-882f5c7f7add"
}</pre>
 



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
    
5. When creating a cluster, you can select previously added recipes on the advanced **Cluster Extensions** page of the create cluster wizard. 


### Reusable recipes 

#### Install mysql connector recipe

Starting from Ambari version 2.6, if you have 'MYSQL_SERVER' component in your blueprint, then you have to [manually install and register](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.2.0/bk_ambari-administration/content/using_hive_with_mysql.html) the 'mysql-connector-java.jar'.

If you would like to automate this process in Cloudbreak:

* Review the recipe content to ensure that the version of the connector provided in the recipe is as desired; if it is not adjust the version.     
* Apply the recipe as "pre-ambari-start".  

The recipe content is:

<pre>
#!/bin/bash

download_mysql_jdbc_driver() {
  wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.39.tar.gz -P /tmp
  tar xzf /tmp/mysql-connector-java-5.1.39.tar.gz -C /tmp/
  cp /tmp/mysql-connector-java-5.1.39/mysql-connector-java-5.1.39-bin.jar /opt/jdbc-drivers/mysql-connector-java.jar
}

main() {
  download_mysql_jdbc_driver
}

main
</pre>



## Using management packs

Management packs allow you to deploy a range of services to your Ambari-managed cluster. You can use a management pack to deploy a specific component or service, such as HDP Search, or to deploy an entire platform, such as HDF.

Cloudbreak supports using management packs, allowing you to register them in Cloudbreak web UI and CLI and then select to install them as part of cluster creation. 

For general information on management packs, refer to [Ambari cwiki: Management+Packs](https://cwiki.apache.org/confluence/display/AMBARI/Management+Packs).  

**Related Links**  
[Ambari cwiki: Management+Packs](https://cwiki.apache.org/confluence/display/AMBARI/Management+Packs)  


### Add management pack

In order to have a management stack installed for a specific cluster, you must register it with Cloudbreak by using the steps below.

**Steps**

1. Obtain the URL for the management pack tarball file that you want to register in Cloudbreak. The tarball  must be available in a location accessible to clusters created by Cloudbreak.  

1. In Cloudbreak web UI, select **External Sources > Management Packs** from the navigation menu. 

3. Click on **Register Management Pack**. 

4. Provide the following:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your management pack. |
| Description | (Optional) Enter a description.|
| Management pack URL | Provide the URL to the location where the management pack tarball file is available. |
| Remove all existing Ambari stack definitions prior to installing this Management Pack (“mpack --purge”).
| Checking this option allows you to purge any existing stack definition and should be included only when installing a stack management pack. Do not select this when installing an add-on service management pack. |
    
4. When creating a cluster, on the advanced **Cluster Extensions** page of the create cluster wizard, you can select one or more previously registered management packs. After selecting, click **Install** to use the management pack for the cluster. 




## Enabling Kerberos security

When creating a cluster, you can optionally enable Kerberos security in that cluster and provide your Kerberos configuration details. Cloudbreak will automatically extend your blueprint configuration with the defined properties.
	
### Kerberos overview 

Kerberos is a third party authentication mechanism, in which users and services that users wish to access Hadoop rely on a third party - the Kerberos server - to authenticate each to the other. The Kerberos server itself is known as the **Key Distribution Center**, or **KDC**. At a high level, the KDC has three parts:

* A database of the users and services (known as **principals**) and their respective Kerberos passwords  
* An **Authentication Server (AS)** which performs the initial authentication and issues a Ticket Granting Ticket (TGT)  
* A **Ticket Granting Server (TGS)** that issues subsequent service tickets based on the initial TGT  

A user principal requests authentication from the AS. The AS returns a TGT that is encrypted using the user principal's Kerberos password, which is known only to the user principal and the AS. The user principal decrypts the TGT locally using its Kerberos password, and from that point forward, until the ticket expires, the user principal can use the TGT to get service tickets from the TGS. Service tickets are what allow the principal to access various services. 

Since cluster resources (hosts or services) cannot provide a password each time to decrypt the TGT, they use a special file, called a **keytab**, which contains the resource principal authentication credentials. The set of hosts, users, and services over which the Kerberos server has control is called a **realm**.

The following table explains the Kerberos related terminology:

| Term | Description |
|---|---|
| Key Distribution Center, or KDC | The trusted source for authentication in a Kerberos-enabled environment. |
| Kerberos KDC Server | The machine, or server, that serves as the Key Distribution Center (KDC). |
| Kerberos Client | Any machine in the cluster that authenticates against the KDC. |
| Principal | The unique name of a user or service that authenticates against the KDC. |
| Keytab | A file that includes one or more principals and their keys.
| Realm | The Kerberos network that includes a KDC and a number of clients. |

### Enabling Kerberos  

The option to enable Kerberos is available in the advanced **Security** section of the create cluster wizard.  

You have the following options for enabling Kerberos in a Cloudbreak  managed cluster:

| Option | Description | Environment |
|---|---|---|
| [Use existing KDC](#using-existing-kdc) | <p>Allows you to leverage an existing MIT KDC or Active Directory for enabling Kerberos with the cluster.<p/><p>You can either provide the required parameters and Cloudbreak will generate the descriptors on your behalf, or provide the exact Ambari Kerberos descriptors to be injected into your blueprint in JSON format.</p> | Suitable for production |
| [Use test KDC](#using-test-kdc) | <p>Installs a new MIT KDC on the master node and configures the cluster to leverage that KDC.</p> | Suitable for evaluation and testing only, not suitable for production |

#### Using existing KDC 

To use an existing KDC, in the advanced **Security** section of the create cluster wizard select **Enable Kerberos Security**. By default, **Use Existing KDC** option is selected.  

You must provide the following information about your MIT KDC or Active Directory. Based on these parameters, kerberos-env and krb5-conf JSON descriptors for Ambari are generated and injected into your Blueprint:

> Before proceeding with the configuration, you must confirm that you met the requirements by checking the boxes next to all requirements listed. The configuration options are displayed only after you have confirmed all the requirements by checking every box.    

| Parameter | Description |
|---|---|
| Kerberos Admin Principal | The admin principal in your existing MIT KDC or AD. | 
| Kerberos Admin Password  | The admin principal password in your existing MIT KDC or AD. | 
| MIT KDC or Active Directory | Select MIT KDC or Active Directory. |

**Use basic configuration**

| Parameter | Required if using... | Description |
|---|---|---|
| Kerberos Url | MIT, AD | IP address or FQDN for the KDC host. Optionally a port number may be included. Example: "kdc.example1.com:88" or "kdc.example1.com" |
| Kerberos Admin URL | MIT, AD | (Optional) IP address or FQDN for the KDC admin host. Optionally a port number may be included. Example: "kdc.example2.com:88" or "kdc.example2.com" |
| Kerberos Realm | MIT, AD | The default realm to use when creating service principals. Example: "EXAMPLE.COM" |
| Kerberos AD Ldap Url | AD | The URL to the Active Directory LDAP Interface. This value must indicate a secure channel using LDAPS since it is required for creating and updating passwords for Active Directory accounts. Example: "ldaps://ad.example.com:636" |
| Kerberos AD Container DN | AD | The distinguished name (DN) of the container used store service principals. Example:  "OU=hadoop,DC=example,DC=com" |
| Use TCP Connection | Optional | By default, Kerberos uses UDP. Checkmark this box to use TCP instead. |

**Use advanced configuration** 

Checking the **Use Custom Configuration** option allows you to provide the actual Ambari Kerberos descriptors to be injected into your blueprint (instead of Cloudbreak generating the descriptors on your behalf). This is the most powerful option which gives you full control of the Ambari Kerberos options that are available. You must provide: 

* Kerberos-env JSON Descriptor (required)
* krb5-conf JSON Descriptor (optional)

To learn more about the Ambari Kerberos JSON descriptors, refer to [Apache cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Automated+Kerberizaton#AutomatedKerberizaton-Configurations).  


#### Using test KDC 

To use a test KDC, in the advanced **Security** section of the create cluster wizard select **Enable Kerberos Security** and then select **Use Test KDC**.

> Using the Test KDC is for evaluation and testing purposes only, and cannot be used for production clusters. To enable Kerberos for production use, you must use the *Use Existing KDC* option. 
 
You must provide the following parameters for your new test KDC:  

| Parameter | Description |
|---|---|
| Kerberos Master Key | The master key for the KDC database. | 
| Kerberos Admin Username | The admin principal to create that can administer the KDC. |
| Kerberos Admin Password | The admin principal password. |
| Confirm Kerberos Admin Password | The admin principal password. |  

When using the test KDC option:
    
* Cloudbreak installs an MIT KDC instance on the Ambari server node.  
* Kerberos clients are installed on all cluster nodes, and the krb5.conf is configured to use the MIT KDC.  
* The cluster is configured for Kerberos to use the MIT KDC. Very basic Ambari KSON Kerberos descriptors are generated and used accordingly.


Example kerberos-env JSON descriptor file:

<pre>{
      "kerberos-env" : {
        "properties" : {
          "kdc_type" : "mit-kdc",
          "kdc_hosts" : "ip-10-0-121-81.ec2.internal",
          "realm" : "EC2.INTERNAL",
          "encryption_types" : "aes des3-cbc-sha1 rc4 des-cbc-md5",
          "ldap_url" : "",
          "admin_server_host" : "ip-10-0-121-81.ec2.internal",
          "container_dn" : ""
        }
      }
    }</pre> 
    
Example krb5-conf JSON  descriptor file: 

<pre> {
      "krb5-conf" : {
        "properties" : {
          "domains" : ".ec2.internal",
          "manage_krb5_conf" : "true"
        }
      }
    }</pre>  


**Related links**   
[Apache cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Automated+Kerberizaton#AutomatedKerberizaton-Configurations) 



## Using an external database for cluster services  

Cloudbreak allows you to register an existing RDBMS instance as an [external source](https://hortonworks.github.io/cloudbreak-documentation/latest/) to be used for a database for certain services. After you register the RDBMS with Cloudbreak, you can use it for multiple clusters. 

### Supported databases
 
If you would like to use an external database for one of the components that support it, you may use the following database types and versions: 

| Component | Supported databases | Description |
|---|---|---|---|---|
| **Ambari**   | PostgreSQL, MySQL  | By default, Ambari uses an embedded PostgreSQL instance.  |
| **Druid**    | PostgreSQL, MySQL  | You must provide an external database. |
| **Hive**     | PostgreSQL, MySQL, Oracle 11, Oracle 12 | By default, Cloudbreak installs a PostgreSQL instance on the Hive Metastore host.  |
| **Oozie**    | PostgreSQL, MySQL, Oracle 11, Oracle 12 | You must provide an external database. |
| **Ranger**   | PostgreSQL, MySQL, Oracle 11, Oracle 12 | You must provide an external database. |
| **Superset** |PostgreSQL, MySQL | You must provide an external database. |
| **Other** | PostgreSQL, MySQL, Oracle 11, Oracle 12 | |

[Comment]: <> (During the demo, Richard mentioned that in addition to these, Ambari supports Oracle; however, in CLoudbreak we have a problem automating one of the steps (related to Oracle tools). This issue is pending, but as of April 16 we cannot support Oracle for Ambari.)

[Comment]: <> (Also Attila mentioned that there were some problems with Ranger. Did anything change here that may need to be documented?)

### External database options  

Cloudbreak includes the following external database options:

| Option | Description | Blueprint requirements | Steps | Example |
|---|---|---|---|---|
| **Built-in types** | Cloudbreak includes a few built-in types: Hive, Druid, Ranger, Superset, and Oozie. | Use a standard blueprint which does not include any JDBC  parameters. Cloudbreak automatically injects the JDBC property variables into the blueprint. | Simply [register the database in the UI](#register-an-external-database). After that, you can attach the database config to your clusters. | Refer to [Example 1](#example-1-built-in-type-hive) |
| **Other types** | In addition to the built-in types, Cloudbreak allows you to specify custom types. In the UI, this corresponds to the UI option is called "Other" > "Enter the type". | You must provide a custom dynamic blueprint which includes RDBMS-specific variables. Refer to [Creating a template blueprint](https://hortonworks.github.io/cloudbreak-documentation/latest/). | Prepare your custom blueprint first. Next, [register the database in the UI](#register-an-external-database). After that, you can attach the database config to your clusters. | Refer to [Example 2](#example-2-other-type) | 

During cluster create, Cloudbreak checks whether the JDBC properties are present in the blueprint:

<img src="../images/cb_cb-rdbms-diagram.png" width="550" title="Cloudbreak web UI">

[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)  

  
#### Example 1: Built-in type Hive 

In this scenario, you start up with a standard blueprint, and Cloudbreak injects the JDBC properties into the blueprint.

1. Register an existing external database of "Hive" type (built-in type):

    <img src="../images/cb_cb-rdbms-e1.png" width="550" title="Cloudbreak web UI">
    
[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)
    
    | Property variable | Example value |
|---|---|
| rds.hive.connectionString | jdbc:postgresql://ec2-54-159-202-231.compute-1.amazonaws.com:5432/hive |
| rds.hive.connectionDriver | org.postgresql.Driver | 
| rds.hive.connectionUserName | myuser |
| rds.hive.connectionPassword | Hadoop123! |
| rds.hive.fancyName | PostgreSQL |
| rds.hive.databaseType | postgres |

[Comment]: <> (Is the rds.hive.connectionDriver parameter deprecated?) 

2. Create a cluster by using a standard blueprint (i.e. one without JDBC related variables) and by attaching the external Hive database configuration.  

3. Upon cluster create, Hive JDBC properties will be injected into the blueprint according to the following template:

    <pre>...
"hive-site": {
    "properties": {
      "javax.jdo.option.ConnectionURL": "{{{ rds.hive.connectionString }}}",
      "javax.jdo.option.ConnectionDriverName": "{{{ rds.hive.connectionDriver }}}",
      "javax.jdo.option.ConnectionUserName": "{{{ rds.hive.connectionUserName }}}",
      "javax.jdo.option.ConnectionPassword": "{{{ rds.hive.connectionPassword }}}"
    }
  },
  "hive-env" : {
    "properties" : {
      "hive_database" : "Existing {{{ rds.hive.fancyName}}} Database",
      "hive_database_type" : "{{{ rds.hive.databaseType }}}"
    }
}
...</pre> 

[Comment]: <> (Is the rds.hive.connectionDriver parameter deprecated?) 


#### Example 2: Other type 

In this scenario, you start up with a special blueprint including JDBC property variables, and Cloudbreak replaces JDBC-related property variables in the blueprint. 

1. Prepare a blueprint blueprint that includes property variables. Use [mustache template](https://mustache.github.io/) syntax. For example:

    <pre>...
"test-site": {
      "properties": {
       "javax.jdo.option.ConnectionURL":"{{rds.test.connectionString}}"
      }
...</pre>


1. Register an existing external database of some "Other" type. For example:

    <img src="../images/cb_cb-rdbms-e2.png" width="550" title="Cloudbreak web UI">
    
[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)
    
    | Property variable | Example value |
|---|---|
| rds.hive.connectionString| ec2-54-159-202-231.compute-1.amazonaws.com:5432/hive |
| rds.hive.connectionDriver | org.postgresql.Driver | 
| rds.hive.connectionUserName | myuser |
| rds.hive.connectionPassword | Hadoop123! |
| rds.hive.subprotocol | postgres |
| rds.hive.databaseEngine | POSTGRES |


2. Create a cluster by using your custom blueprint and by attaching the external database configuration.  

3. Upon cluster create, Cloudbreak replaces JDBC-related property variables in the blueprint. 

**Related links**  
[Mustache template syntax](https://mustache.github.io/)  

[Comment]: <> (Are these examples still correct or did some of the parameters change in this release?) 
  
  
### Creating a template blueprint for RDMBS  

In order to use an external RDBMS for some component other than the built-in components, you must include JDBC property variables in your blueprint. You must use [mustache template](https://mustache.github.io/) syntax. See [Example 2: Other type](#example-2-other-type) for an example configuration. 



[Comment]: <> (Does this blueprint doc have to be updated to include "Connector's JAR URL" parameter when using MySQL or Oracle?)

[Comment]: <> (Based on the CLI, OI suspect that "rds.[type].connectionURL" may be deprecated now?)


### Register an external database    
  
You must create the external RDBMS instance and database prior to registering it with Cloudbreak. Once you have it ready, you can:

1. Register it in Cloudbreak web UI or CLI.  
2. Use it with one or more clusters. Once registered, the database will now show up in the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**.  

**Prerequisites**  

If you are planning to use an external MySQL or Oracle database, you must download the JDBC connector's JAR file and place it in a location available to the cluster host on which Ambari is installed. The steps below require that you provide the URL to the JDBC connector's JAR file. 

> If you are using your own [custom image](https://hortonworks.github.io/cloudbreak-documentation/latest/), you may place the JDBC connector's JAR file directly on the machine as part of the image burning process. 

**Steps** 

1. From the navigation pane, select **External Sources** > **Database Configurations**.  
2. Select **Register Database Configuration**.    
3. Provide the following information:

    | Parameter | Description |
    |:---|:---|
    | Name | Enter the name to use when registering this database to Cloudbreak. This is **not** the database name. |
    | Type | Select the service for which you would like to use the external database. If you selected "Other", you must provide a special blueprint. |
    | JDBC Connection | Select the database **type** and enter the **JDBC connection** string (HOST:PORT/DB_NAME).  |
    | Connector's JAR URL | (MySQL and Oracle only) Provide a URL to the JDBC connector's JAR file. The JAR file must be hosted in a location accessible to the cluster host on which Ambari is installed. At cluster creation time, Cloudbreak places the JAR file in the /opts/jdbc-drivers directory. You do not need to provide the "Connector's JAR URL if you are using a custom image and the JAR file was either manually placed on the VM as part of custom image burning or it was placed there by using a recipe. |
    | Username | Enter the JDBC connection username. |
    | Password | Enter the JDBC connection password. |

4. Click **Test Connection** to validate and test the RDS connection information.  
5. Once your settings are validated and working, click **REGISTER** to save the configuration.  
6. The database will now show up on the list of available databases when creating a cluster under advanced **External Sources** > **Configure Databases**. You can select it and click **Attach** each time you would like to use it for a cluster.  



## Using an external authentication source for clusters

Cloudbreak allows you to register an existing LDAP/AD instance as an [external source](https://hortonworks.github.io/cloudbreak-documentation/latest/) and use it for multiple clusters. You must create the LDAP/AD prior to registering it with Cloudbreak. Once you have it ready, the overall steps are:
   
1. Register an existing LDAP in Cloudbreak web UI or CLI.  
1. Once registered, the LDAP will now show up in the list of available authentication sources when creating a cluster under advanced **External Sources** > **Configure Authentication**.  
1. Prepare a blueprint as described in [Preparing a blueprint for LDAP/AD ](#preparing-a-blueprint-for-ldapad).  
1. Create a cluster by using the blueprint and by attaching the authentication source. Cloudbreak automatically injects the LDAP property variables into the blueprint. 


### Preparing a blueprint for LDAP/AD 

In order to use LDAP/AD for your cluster, you must provide a suitable cluster blueprint:

- The blueprint must include one or more of the following supported components: Atlas, Hadoop, Hive LLAP, Ranger Admin, Ranger UserSync.  
- The blueprint should not include any LDAP properties. Before injecting the properties, Cloudbreak checks if LDAP related properties already exist in the blueprint. If they exist, they are not injected.  

During cluster creation the following properties will be injected in the blueprint:

- ldap.connectionURL  
- ldap.domain  
- ldap.bindDn  
- ldap.bindPassword  
- ldap.userSearchBase  
- ldap.userObjectClass  
- ldap.userNameAttribute  
- ldap.groupSearchBase  
- ldap.groupObjectClass  
- ldap.groupNameAttribute  
- ldap.groupMemberAttribute  
- ldap.directoryType  
- ldap.directoryTypeShort  

Their values will be the values that you provided to Cloudbreak: 

<img src="../images/cb_cb-ldap.png" width="550" title="Cloudbreak web UI">

[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)


### Register an authentication source 

Cloudbreak allows you to register an existing LDAP/AD instance and use it for multiple clusters. You must create the LDAP/AD prior to registering it with Cloudbreak. Once you have it ready, you can:

1. Register an existing LDAP in Cloudbreak web UI or CLI.  
2. Use it as an authentication source for your clusters. Once registered, the LDAP will now show up in the list of available authentication sources when creating a cluster under advanced **External Sources** > **Configure Authentication**.   

**Steps**

1. From the navigation pane, select **External Sources** > **Authentication Configurations**.  
2. Select **Register Authentication Source**.     
3. Provide the following parameters related to your existing LDAP/AD: 
    
    **GENERAL CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| Name |  Enter a name for your LDAP. | cb-ldap |
| Directory Type | Choose whether your directory is **LDAP** or **Active Directory**. | LDAP |
| LDAP Server Connection | Select **LDAP** or **LDAPS**. | LDAP |
| Server Host | Enter the hostname for the LDAP or AD server . |`10.0.3.128`|
| Server Port | Enter the port. | `389` |
| LDAP Bind DN | Enter the root Distinguished Name to search in the directory for users. | `CN=Administrator,CN=Users,DC=ad,DC=hdc,DC=com`   |
| LDAP Bind Password | Enter your root Distinguished Name password.  | `MyPassword1234!` |

    **USER CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| LDAP User Search Base | Enter your LDAP user search base. This defines the location in the directory from which the LDAP search begins. | `CN=Users,DC=ad,DC=hdc,DC=com`  |
| LDAP User Name Attribute | Enter the attribute for which to conduct a search on the user base.  | `HDCaccountName` |
| LDAP User Object Class | Enter the directory object class for users. | `person` |

    **GROUP CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| LDAP Group Search Base | Enter your LDAP group search base. This defines the location in the directory from which the LDAP search begins. | `OU-scoDC=ad,DC=hdc,DC=com`  |
| LDAP Admin Group | (Optional) Enter your LDAP admin group, if needed. |  |
| LDAP Group Name Attribute | Enter the attribute for which to conduct a search on groups.  | `cn` |
| LDAP Group Object Class | Enter the directory object class for groups. | `group`  |
| LDAP Group Member Attribute | Enter the attribute on the group object class that represents members. | `member` |

5. Click **Test Connection** to verify that the connection information that you entered is correct.
 
6. Click **REGISTER**. 

7. The LDAP will now show up on the list of available authentication sources when creating a cluster under advanced **External Sources** > **Configure Authentication**. It can be reused with multiple clusters. Just select it if you would like to use it for a given cluster.     





## Register a proxy   

Cloudbreak allows you to save your existing proxy configuration information as an [external source](https://hortonworks.github.io/cloudbreak-documentation/latest/) so that you can provide the proxy information to multiple clusters that you create with Cloudbreak. The steps are:       

1. Register your proxy in Cloudbreak web UI or CLI.   
2. Once the proxy has been registered with Cloudbreak, it will show up in the list of available proxies when creating a cluster under advanced **External Sources** > **Configure Proxy**.  


**Steps** 

1. From the navigation pane, select **External Sources** > **Proxy Configurations**.  
2. Select **Register Proxy Configuration**.    
5. Provide the following information:

    | Parameter | Description | Example |
    |:---|:---|:---|
    | Name | Enter the name to use when registering this database to Cloudbreak. This is **not** the database name. | my-proxy |
    | Description | Provide description. | |
    | Protocol | Select HTTP or HTTPS. | HTTPS|
    | Server Host| Enter the URL of your proxy server host. | 10.0.2.237 |
    | Server Port | Enter proxy server port. | 3128 |
    | Username | Enter the username for the proxy. | testuser |
    | Password | Enter the password for the proxy. | MyPassword123 |

6. Click **REGISTER** to save the configuration. 

7. The proxy will now show up when creating a cluster under advanced **External Sources** > **Configure Proxy**. You can select it each time you would like to use it for a cluster.  
  


## Setting up a data lake

> This feature is technical preview.  
 
A **data lake** provides a way for you to centrally apply and enforce authentication, authorization, and audit policies across multiple ephemeral workload clusters. "Attaching" your workload cluster to the data lake instance allows the attached cluster workloads to access data and run in the security context provided by the data lake. 

While workloads are temporary, the security policies around your data schema are long-running and shared for all workloads. As your workloads come and go, the data lake instance lives on, providing consistent and available security policy definitions that are available for current and future ephemeral workloads. All information related to schema (Hive), security policies (Ranger) and audit (Ranger) is stored on external locations (external databases and cloud storage).

This is illustrated in the following diagram: 

<img src="../images/cb_datalake-diag01.png" width="550" title="Cloudbreak architecture">

[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)

Once you’ve created a data lake instance, you have an option to attach it to one or more ephemeral clusters. This allows you to apply the authentication, authorization, and audit across multiple workload clusters.

The following table explains basic data lake terminology: 

| Term | Description |
|---|---|
| Data lake | Runs Ranger, which is used for configuring authorization. policies and is used for audit capture. Runs Hive Metastore, which is used for data schema. |
| Workload clusters | The clusters that get attached to the data lake to run workloads. This is where you run workloads such as Hive via [JDBC](https://hortonworks.github.io/cloudbreak-documentation/latest/). |

The following table explains the components of a data lake: 

<img src="../images/cb_datalake-diag02.png" width="550" title="Cloudbreak architecture">

[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)

| Component | Technology | Description |
|---|---|---|
| Schema | Apache Hive | Provides Hive schema (tables, views, and so on). If you have two or more workloads accessing the same Hive data, you need to share schema across these workloads. |
| Policy | Apache Ranger | Defines security policies around Hive schema. If you have two or more users accessing the same data, you need security policies to be consistently available and enforced. |
| Audit | Apache Ranger | Audits user access and captures data access activity for the workloads. |
| [Directory ](https://hortonworks.github.io/cloudbreak-documentation/latest/)| LDAP/AD | Provides an authentication source for users and a definition of groups for authorization. |
| [Gateway](https://hortonworks.github.io/cloudbreak-documentation/latest/) | Apache Knox | Supports a single workload endpoint that can be protected with SSL and enabled for authentication to access to resources. |

### Overview of data lake setup 

Setting up a data lake involves the following steps:

| Step | Where to perform |
|---|---|
| [Meet the prerequisites](#prerequisites):<ul><li>Create two external databases (one for Hive metastore and one for Ranger)</li><li>Create an external authentication source for LDAP/AD</li><li>Prepare a cloud storage location (depending on your cloud provider, this should be, on Amazon S3, Azure's ADLS or WASB, or GCS) for default Hive warehouse directory and Ranger audit logs)</li></ul>|You must create these resources on your own, outside of Cloudbreak. You may use one database instance and create two databases.|
| [Register the two databases and LDAP](#register-the-databases-and-ldap-in-the-cloudbreak-web-ui) | In the Cloudbreak web UI > External Sources |
| [Create a data lake](#create-a-data-lake) | In the Cloudbreak web UI > Create cluster |
| [Create clusters attached to the data lake](#create-attached-hdp-clusters) | In the Cloudbreak web UI > Create cluster |

### Prerequisites

You must perform the following prerequisites *outside of Cloudbreak*: 

1. Set up two external database instances, one for the HIVE component, and one for the RANGER component. For supported databases, refer to [Using an external database](https://hortonworks.github.io/cloudbreak-documentation/latest/).     
2. Create an LDAP instance and set up your users inside the LDAP.  
3. Prepare a cloud storage location for default Hive warehouse directory and Ranger audit logs.  

In the steps that follow, you will be required to provide the information related to these external resources. 

### Register the databases and LDAP in the Cloudbreak web UI

Prior to creating a data lake, you must register the following resources in the Cloudbreak web UI:

**Steps** 

1. Register each of your two RDS instances created as part of the prerequisites in the Cloudbreak web UI, under **External Sources** > **Database Configurations**. For instructions, refer  [Using an external database](https://hortonworks.github.io/cloudbreak-documentation/latest/).    
    * When registering the database for Hive, select **Type > Hive**.  
    * When registering the database for Ranger, select **Type > Ranger**.  

2. Register your LDAP (created as part of the prerequisites) in the Cloudbreak web UI, under **External Sources > Authentication Configurations**. For instructions, refer to [Register an authentication source](https://hortonworks.github.io/cloudbreak-documentation/latest/).  

As an outcome of this step, you should have two external databases and one authentication source registered in the Cloudbreak web UI.  


### Create a data lake 

Create a data lake by using the create cluster wizard. Among other information, make sure to provide the information listed in the steps below. 

**Steps** 

1. Navigate to the advanced version of the create cluster wizard.  
2. On the **General Configuration** page:  
    * Under **Cluster Name**, provide a name for your data lake. 
    * Under **Cluster Type**, choose the **DATA LAKE** blueprint:

    <img src="../images/cb_datalake01.png" width="650" title="Cloudbreak web UI">   
    
3. On the **Cloud Storage** page:  
    * Under **Cloud Storage**, configure access to cloud storage via the method available for your cloud provider.  
    * Under **Storage Locations**, provide an existing location within your cloud storage account that can be used for Hive warehouse directory and Ranger audit logs.  

    > The storage location must exist prior to data lake provisioning. If the storage location does not exist then Ranger is installed properly, but it may not work.

    <img src="../images/cb_datalake02.png" width="650" title="Cloudbreak web UI">  
   
4. On the **External Sources** page, select the previously registered Ranger database, Hive database and LDAP:

    <img src="../images/cb_datalake03.png" width="650" title="Cloudbreak web UI"> 
    
5. On the **Gateway Configuration** page, the gateway is enabled by default with Ambari exposed through the gateway. You should also enable Ranger by selecting the Ranger service and clicking **Expose**. 

    <img src="../images/cb_datalake04.png" width="650" title="Cloudbreak web UI">    
    
6. On the **Security** page:
    * Under **Password**, provide a strong password for your cluster. For example “SomeRandomChars123!” is a strong password. A strong password is required for the default Ranger admin, which - among other cluster components like Ambari - will use this password. 
    * Select an SSH key.

7. Click **CREATE CLUSTER** to initiate data lake creation.       
 
As an outcome of this step, you should have a running data lake.


### Create attached HDP clusters

Once your data lake is running, you can start creating clusters attached to the data lake. Follow these general steps to create an cluster attached to a data lake. 

In general, once you've selected the data lake that the cluster should be using, the cluster wizard should provide you with the cluster settings that should be used for the attached cluster.    

**Steps** 

1. In the Cloudbreak web UI, click on the cluster tile representing your data lake.  

2. From the **ACTIONS** menu, select **CREATE ATTACHED CLUSTER**.    
 
    <img src="../images/cb_datalake05.png" width="350" title="Cloudbreak web UI"> 

3. In general, the cluster wizard should provide you with the cluster settings that should be used for the attached cluster. Still, make sure to do the following:

    * Under **Region** and **Availability Zone**, select the same location where your data lake is running.   
    * Select one of the three default blueprints.  
    * On the **Cloud Storage** page, enter the same cloud storage location that your data lake is using.  
    * On the **External Sources** page, the LDAP, and Ranger and Hive databases that you attached to the data lake should be attached to your cluster.      
    * On the **Network** page, select the same VPC and subnet where the data lake is running.  

4. Click on **CREATE CLUSTER** to initiate cluster creation. 

As an outcome of this step, you should have a running cluster attached to the data lake. Access your attached clusters and run your workloads as normal.





## Using custom hostnames based on DNS

By default, when Cloudbreak provisions cloud provider resources, your cloud provider assigns hostnames for your cluster nodes. Optionally, instead of using default hostnames, you can configure Cloudbreak to use custom hostnames based on DNS. To do that, follow the steps for configuring reverse Domain Name System (DNS) described below. 

When the cluster node machines are provisioned, they try to make a reverse DNS lookup (by querying of the DNS to determine the domain name associated with a specific IP address); if the reverse DNS lookup returns a valid value, then that value is set as hostname. This is why reverse DNS setup is required for using custom hostnames.   

### Configure DNS on AWS

On AWS you have the following two options:

* Use [Route53](https://aws.amazon.com/route53/) as DNS provider.  
* Set up your own DNS server in your VPC  

Both options require you to have an existing VPC and attach a custom [DHCP option](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_DHCP_Options.html) to it.

The instructions provided in this section describe how to perform the steps in Amazon web consoles, but the steps can also be performed (and automated) in the AWS CLI.  

#### Configure DNS using Route53

Follow these general steps to configure reverse DNS using [Route53](https://aws.amazon.com/route53/). 

**Steps**

1. **Create a new VPC or use your existing VPC**:

    1. You can create a new VPC from the Amazon VCP console (for example by using *Start VPC Wizard*):              
        * CIDR block example: *10.1.0.0/16* 
        * Subnet's CIDR example: *10.1.1.0/28*  
    2. Make sure to:
        * Enable DNS resolution for the VPC. You can do this by selecting the VPC, selecting *Actions > Edit DNS resolution* and choosing *Yes*. 
        * Enable DNS hostnames for the VPC. You can do this by selecting the VPC, selecting *Actions > Edit DNS hostnames* and choosing *Yes*.
    
    > Optionally, you may want to set up an Internet Gateway for the VPC and add a default route to the routing table for the Internet Gateway.
    Additionally, you may want to enable the *Auto-assign Public IP* option.
    This way Cloudbreak would reach the cluster from outside of the VPC and the cluster would have internet access.
    
2. **Create a DHCP options set**: 

    Perform this step from the Amazon VPC console. Select *DHCP Options Sets* from the left pane and click on *Create a DHCP options set*. Make sure to:   
    
    * Set the *Domain name* to a preferred domain, for example `cloudbreak.beer`
    * Set the *Domain name servers* to `AmazonProvidedDNS`  

        <img src="../images/cb_aws-route53-dhcp-option.png" width="650" title="DHCP option for Route53">   

    For detailed steps, refer to [AWS documentation](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/dhcp_options_set.html). 
    
3. **Assign the newly created DHCP options set to your VPC**:

    1. From the Amazon VPC console, select *Your VPCs* from the left pane.
    2. Select the VPC created earlier.  
    3. Click on *Actions > Edit DHCP Options Set*.  
    4. Select the newly created DHCP option set.  
  
4. **Configure your domain at Route53**:

    Perform these steps from the Amazon Route53 console.  
    For general steps, refer to [AWS documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zone-private-creating.html). 
     
    1. Select *Hosted zones* from the left pane.    
    2. Create a hosted zone by clicking on *Create Hosted Zone*. Make sure to:   
        * Use the same domain name as used previously with the DHCP options set (In the example this was `cloudbreak.beer`). 
        * Set the *Type* to *Private Hosted Zone for Amazon VPC*.  
        * Select the VPC ID of the VPC to which you previously assigned the DHCP option.  
        
    3. Add records for your hosted zone:
    
        * Select the hosted zone and choose *Go to Record Sets*
        * Click *Create Record Set* to create a record set. You must perform this step for every available IP, so that each IP can have a custom name (If you used the subnet example listed above, these IPs will be in the range of 10.1.1.4-14):
            * Type: select *A*
            * Name: for example *b10*
            * Value: for example *10.1.1.10*

            <img src="../images/cb_aws-route53-record.png" width="350" title="Domain record for Route53"> 
            
      4. After performing this step for each IP, you should end up with an many records as IPs. For example:  
          <img src="../images/cb_aws-route53-records.png" width="650" title="Domain record for Route53">
                       


5. **Create another hosted zone for reverse DNS lookup.**

    Perform these steps from the Amazon Route53 console.  
     
    1. Select *Hosted zones* from the left pane.    
    2. Create a hosted zone by clicking on *Create Hosted Zone*. Make sure to: 
        * For example, if you used the subnet example listed above, its `Domain name` should look like this (as reverse DNS lookups use the special domain *in-addr.arpa*):

            <pre>1.1.10.in-addr.arpa.</pre>

        * Set the Type to *Private Hosted Zone for Amazon VPC*.
        * Select the VPC ID to which you previously assigned the DHCP option set.  

     3. Add records for every created domain: 
        * Type: select *PTR* 
        * Name: This determines the first part of the IP, for example *10*
        * Value: Enter the domain name that you set in the previous step, for example, *b10*

            <img src="../images/cb_aws-route53-reverse-record.png" width="350" title="Domain PTR record for Route53"></a>
            
      4. After performing this step for each domain, you should end up with as many records as IPs. For example:  
          <img src="../images/cb_aws-route53-reverse-records.png" width="650" title="Domain record for Route53"></a>

4. **Create the cluster in the VPC configured in the earlier steps** and you will have the same hostnames set as the domain names.
> Since you don't have control the order over the IP addresses leased to the machines, the names may not be in order.


#### Configure DNS using custom DNS server

Follow these general steps to configure reverse DNS using a custom DNS server.  

**Steps**

1. **Create a new VPC or use your existing VPC**:

    1. You can create a new VPC from the Amazon VCP console (for example by using *Start VPC Wizard*):              
        * CIDR block example: *10.1.0.0/16* 
        * Subnet's CIDR example: *10.1.1.0/28*  
    2. Make sure to:
        * Enable DNS resolution for the VPC. You can do this by selecting the VPC, selecting *Actions > Edit DNS resolution* and choosing *Yes*. 
        * Enable DNS hostnames for the VPC. You can do this by selecting the VPC, selecting *Actions > Edit DNS hostnames* and choosing *Yes*.
        
    > Optionally, you may want to set up an Internet Gateway for the VPC and add a default route to the routing table for the Internet Gateway.
    Additionally, you may want to enable the *Auto-assign Public IP* option.
    This way Cloudbreak would reach the cluster from outside of the VPC and the cluster would have internet access.
    
2. **Set up DNS server in your VPC/subnet**:
    * In the configuration ensure that you have DNS records and reverse DNS pointers for all IP address (for example 10.3.3.4-14)
    * Example unbound configuration:

        <pre>
            [root@ip-10-3-3-9 conf.d]# cat 00-cloudbreak.cloud.conf
            server:
                local-zone: "cloudbreak.cloud." static
                local-data: "aww1.cloudbreak.cloud. IN A 10.3.3.4"
                local-data-ptr: "10.3.3.4 aww1.cloudbreak.cloud."
                local-data: "aww2.cloudbreak.cloud. IN A 10.3.3.5"
                local-data-ptr: "10.3.3.5 aww2.cloudbreak.cloud."
                local-data: "aww3.cloudbreak.cloud. IN A 10.3.3.6"
                local-data-ptr: "10.3.3.6 aww3.cloudbreak.cloud."
                local-data: "aww4.cloudbreak.cloud. IN A 10.3.3.7"
                local-data-ptr: "10.3.3.7 aww4.cloudbreak.cloud."
                local-data: "aww5.cloudbreak.cloud. IN A 10.3.3.8"
                local-data-ptr: "10.3.3.8 aww5.cloudbreak.cloud."
                local-data: "aww6.cloudbreak.cloud. IN A 10.3.3.9"
                local-data-ptr: "10.3.3.9 aww6.cloudbreak.cloud."
                local-data: "aww7.cloudbreak.cloud. IN A 10.3.3.10"
                local-data-ptr: "10.3.3.10 aww7.cloudbreak.cloud."
                local-data: "aww8.cloudbreak.cloud. IN A 10.3.3.11"
                local-data-ptr: "10.3.3.11 aww8.cloudbreak.cloud."
                local-data: "aww9.cloudbreak.cloud. IN A 10.3.3.12"
                local-data-ptr: "10.3.3.12 aww9.cloudbreak.cloud."
                local-data: "aww10.cloudbreak.cloud. IN A 10.3.3.13"
                local-data-ptr: "10.3.3.13 aww10.cloudbreak.cloud."
                local-data: "aww11.cloudbreak.cloud. IN A 10.3.3.14"
                local-data-ptr: "10.3.3.14 aww11.cloudbreak.cloud."
        </pre>

3. **Create a DHCP options set**: 

    Perform this step from the Amazon VPC console. Select *DHCP Options Sets* from the left pane and click on *Create a DHCP options set*. Make sure to:   
    
    * Set the *Domain name* to your preferred domain, for example `cloudbreak.cloud`
    * Set *Domain name servers* to the previously created DNS server
    * Optionally, set a *Name tag*  

        <img src="../images/cb_aws-custondns-dhcp-option.png" width="650" title="DHCP option for own DNS server"> 
        
    For detailed steps, refer to [AWS documentation](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/dhcp_options_set.html). 

4. **Assign the newly created DHCP options set to your VPC**:

    1. From the Amazon VPC console, select *Your VPCs* from the left pane.
    2. Select the VPC created earlier.  
    3. Click on *Actions > Edit DHCP Options Set*.  
    4. Select the newly created DHCP option set.  


5. **Create the cluster in the VPC configured in the preceding steps** and you will have the same hostnames set as the domain names.
> Since you don't have control the order over the IP addresses leased to the machines, the names may not be in order.







