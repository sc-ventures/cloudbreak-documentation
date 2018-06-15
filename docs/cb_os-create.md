## Creating a cluster on OpenStack 

Use these steps to create a cluster.

> If you experience problems during cluster creation, refer to [Troubleshooting cluster creation](https://hortonworks.github.io/cloudbreak-documentation/latest/).

**Steps**

1. Log in to the Cloudbreak UI.

2. Click the **Create Cluster** button and the *Create Cluster* wizard is displayed.  
    By default, **Basic** view is displayed. To view advanced options, click **Advanced**. To learn about advanced options, refer to [Advanced cluster options](https://hortonworks.github.io/cloudbreak-documentation/latest/).

    <img src="../images/cb_cb-create.png" width="650" title="Cluster Information"> 

3. On the **General Configuration** page, specify the following general parameters for your cluster:

    | Parameter | Description |
|---|---|
| Select Credential | Choose a previously created credential. |
| Cluster Name | Enter a name for your cluster. The name must be between 5 and 40 characters, must start with a letter, and must only include lowercase letters, numbers, and hyphens. |
| Region | Select the region in which you would like to launch your cluster. |
| Platform Version | Choose the HDP or HDF version to use for this cluster. Blueprints available for this platform version will be populated under "Cluster Type" below. If you selected the **HDF** platform, refer to [Creating HDF clusters](https://hortonworks.github.io/cloudbreak-documentation/latest/) for HDF cluster configuration tips. |
| Cluster Type | Choose one of the default cluster configurations, or, if you have defined your own cluster configuration via Ambari blueprint, you can choose it here. For more information on default and custom blueprints, refer to [Using custom blueprints](https://hortonworks.github.io/cloudbreak-documentation/latest/). |
| Flex Subscription | This option will appear if you have configured your deployment for a [flex support subscription](https://hortonworks.github.io/cloudbreak-documentation/latest/). |

4. On the **Hardware and Storage** page, for each host group provide the following information to define your cluster nodes and attached storage. 

    To edit this section, click on the <img src="../images/cb_edit.png"/>. When done editing, click on the <img src="../images/cb_save.png" width="25"/> to save the changes.
    
    | Parameter | Description |
|---|---|
| Ambari Server | You must select one node for Ambari Server by clicking the <img src="../images/cb_toggle.png" alt="On"/> button. The "Instance Count" for that host group must be set to "1". If you are using one of the default blueprints, this is set by default. | 
| Instance Type | Select an instance type. |
| Instance Count | Enter the number of instances of a given type. Default is 1. |  
| Storage Type | <p>Select the volume type. The options are:<ul><li>Magnetic</li><li>Ephemeral</li><li>General Purpose (SSD)</li><li>Throughput Optimized HDD</li></ul>For more information about these options refer to <a href="http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html" target="_blank">AWS documentation</a>.</p>|
| Attached Volumes Per Instance | Enter the number of volumes attached per instance. Default is 1. |
| Volume Size | Enter the size in GBs for each volume. Default is 100. |

6. On the **Gateway Configuration** page, you can access gateway configuration options. 

    When creating a cluster, Cloudbreak installs and configures a gateway (powered by Apache Knox) to protect access to the cluster resources. By default, the gateway is enabled for Ambari; You can optionally enable it for other cluster services. 
    
    For more information, refer to [Configuring the Gateway](https://hortonworks.github.io/cloudbreak-documentation/latest/) documentation.

6. On the **Network** page, provide the following to specify the networking resources that will be used for your cluster:

    | Parameter | Description |
|---|---|
| Select Network | Select the virtual network in which you would like your cluster to be provisioned. You can select an existing network or create a new network. |
| Select Subnet | Select the subnet in which you would like your cluster to be provisioned. If you are using a new network, create a new subnet. If you are using an existing network, select an existing subnet. |
| Subnet (CIDR)| If you selected to create a new subnet, you must define a valid [CIDR](http://www.ipaddressguide.com/cidr) for the subnet. Default is 10.0.0.0/16. |

    > Cloudbreak uses public IP addresses when communicating with cluster nodes.  


7. Define security groups for each host group. You can either create new security groups and define their rules or reuse existing security groups:


[Comment]: <> (On AWS/GCP Existing security groups should be selectable only for existing VPC.)

    | Option | Description |
|---|---|
| New Security Group | <p>(Default) Creates a new security group with the rules that you defined:</p><p><ul><li>A set of [default rules](https://hortonworks.github.io/cloudbreak-documentation/latest/) is provided. You should review and adjust these default rules. If you do not make any modifications, default rules will be applied. </li><li>You may open ports by defining the CIDR, entering port range, selecting protocol and clicking **+**.</li><li>You may delete default or previously added rules using the delete icon.</li><li>If you don't want to use security group, remove the default rules.</li><ul></p> |  
| Existing Security Groups | Allows you to select an existing security group that is already available in the selected provider region. This selection is disabled if no existing security groups are available in your chosen region. |  

    The default experience of creating network resources such as network, subnet and security group automatically is provided for convenience. We strongly recommend you review these options and for production cluster deployments leverage your existing network resources that you have defined and validated to meet your enterprise requirements. For more information, refer to <a href="../security-cb-inbound/index.html">Restricting inbound access from Cloudbreak to cluster</a>. 


5. On the **Security** page, provide the following parameters:

    | Parameter | Description |
|---|---|
| Cluster User | You can log in to the Ambari UI using this username. By default, this is set to `admin`. |
| Password | You can log in to the Ambari UI using this password. |
| Confirm Password | Confirm the password. |
| New SSH public key | Check this option to specify a new public key and then enter the public key. You will use the matching private key to access your cluster nodes via SSH. |
| Existing SSH public key | Select an existing public key. You will use the matching private key to access your cluster nodes via SSH. This is a default option as long as an existing SSH public key is available.  |


8. Click on **Create Cluster** to create a cluster.

9. You will be redirected to the Cloudbreak dashboard, and a new tile representing your cluster will appear at the top of the page.
  

### Creating HDF clusters 

In general, the create cluster wizard offers prescriptive default settings that help you configure your HDF clusters properly; however, there are a few additional configuration requirements that you should be aware of. 

#### Creating HDF Flow Management clusters

When creating a Flow Management cluster from the default blueprint, make sure to do the following:

* On the **Hardware and Storage** page, place the **Ambari Server** on the "Services" host group. 
* On the **Network** page, open the required ports:        
    * Open **9091** TCP port on the NiFi host group. This port is used by **NiFi web UI**; Without it, you will be unable to access the NiFi web UI.   
    * Open **61443** TCP port on the Services host group. This port is used by **NiFi Registry**.       
* You should either use your **existing LDAP** or enable **Kerberos**.      
    * If using **LDAP**, you must first [register it as an external authentication source](https://hortonworks.github.io/cloudbreak-documentation/latest/) in the Cloudbreak web UI.   
    * If using **Kerberos**, you can either use your own Kerberos (for production) or select for Cloudbreak to create a test KDC (for evaluation only).  
*  When creating a HDF cluster with LDAP, on the **Security** page:
    * You must must specify a **Cluster User** that is a valid user in the LDAP. This is a limitation with NiFi/NiFi Registry. Only one login provider can be configured for those components, and if an LDAP is supplied, then the login provider is set to LDAP; Consequently, this requires that the initial admin be in the given LDAP.  
    * While the passwords don't need to match between the cluster user and the same-named LDAP user, any other components that are used by that cluster user would require the password entered for the cluster user, not the same-named LDAP user.        
* When creating the NiFi Registry controller service in NiFi, the internal hostname must be used, `e.g. https://ip-1-2-3-4.us-west-2.compute.internal:61443`   
* Although Cloudbreak allows cluster scaling (including autoscaling), scaling is not supported by NiFi:       
    * **Downscaling NiFi clusters is not supported** - as it can result in data loss when a node is removed that has not yet processed all the data on that node.       
    * There is a known issue related to upscaling; It is listed in the [Known Issues](https://hortonworks.github.io/cloudbreak-documentation/latest/). 
    

##### Troubleshooting HDF Flow Management cluster creation 

NiFi returns the following error when the Flow Management cluster is set up with an LDAP: 

<pre>Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'authorizer': FactoryBean threw exception on object creation; nested exception is org.apache.nifi.authorization.exception.AuthorizerCreationException: org.apache.nifi.authorization.exception.AuthorizerCreationException: Unable to locate initial admin admin to seed policies
        at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.doGetObjectFromFactoryBean(FactoryBeanRegistrySupport.java:175)
        at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.getObjectFromFactoryBean(FactoryBeanRegistrySupport.java:103)
        at org.springframework.beans.factory.support.AbstractBeanFactory.getObjectForBeanInstance(AbstractBeanFactory.java:1634)
        at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:317)
        at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:197)
        at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:351)
        ... 90 common frames omitted
Caused by: org.apache.nifi.authorization.exception.AuthorizerCreationException: org.apache.nifi.authorization.exception.AuthorizerCreationException: Unable to locate initial admin admin to seed policies
        at org.apache.nifi.authorization.FileAccessPolicyProvider.onConfigured(FileAccessPolicyProvider.java:231)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.nifi.authorization.AccessPolicyProviderInvocationHandler.invoke(AccessPolicyProviderInvocationHandler.java:46)
        at com.sun.proxy.$Proxy72.onConfigured(Unknown Source)
        at org.apache.nifi.authorization.AuthorizerFactoryBean.getObject(AuthorizerFactoryBean.java:143)
        at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.doGetObjectFromFactoryBean(FactoryBeanRegistrySupport.java:168)
        ... 95 common frames omitted
Caused by: org.apache.nifi.authorization.exception.AuthorizerCreationException: Unable to locate initial admin admin to seed policies
        at org.apache.nifi.authorization.FileAccessPolicyProvider.populateInitialAdmin(FileAccessPolicyProvider.java:566)
        at org.apache.nifi.authorization.FileAccessPolicyProvider.load(FileAccessPolicyProvider.java:509)
        at org.apache.nifi.authorization.FileAccessPolicyProvider.onConfigured(FileAccessPolicyProvider.java:222)
        ... 103 common frames omitted</pre>
        
**Cause**:

When creating a HDF cluster with LDAP, on the **Security** page of the create cluster wizard you specified some **Cluster User** that is not a valid user in the LDAP. 
 

**Solution**:

When creating a HDF cluster with LDAP, on the **Security** page of the create cluster wizard, specify a **Cluster User** that is a valid user in the LDAP. 
        

#### Creating HDF Messaging Management clusters

When creating a Messaging Management cluster from the default blueprint, make sure to do the following:

* On the **Hardware and Storage** page, place the **Ambari Server** on the "Services" host group.  
* When creating a cluster, open **3000** TCP port on the Services host group for Grafana.    

  
    

### Advanced cluster options

Click on **Advanced** to view and enter additional configuration options.


#### Availability zone

 Choose one of the availability zones within the selected region. 
 

#### Enable lifetime management 

Check this option if you would like your cluster to be automatically terminated after a specific amount of time (defined as "Time to Live" in minutes). 

[Comment]: <> (Need to Update this based on pending changes and specify: When does Cloudbreak start counting the TTL, does the time when the cluster is stopped count towards this TTL.)


#### Tags

You can optionally add tags, which will help you find your cluster-related resources, such as VMs, in your cloud provider account. 

By default, the following tags are created:

| Tag | Description |
|---|---|
| cb-version | Cloudbreak version |
| Owner | Your Cloudbreak admin email. |
| cb-account-name | Your automatically generated Cloudbreak account name stored in the identity server. |
| cb-user-name | Your Cloudbreak admin email. |

For more information, refer to [Tagging resources](https://hortonworks.github.io/cloudbreak-documentation/latest/).

   

#### Choose image catalog

By default, **Choose Image Catalog** is set to the default image catalog that is provided with Cloudbreak. If you would like to use a different image catalog, you must first create and register it. For complete instructions, refer to [Using custom images](https://hortonworks.github.io/cloudbreak-documentation/latest/).
  


#### Choose image type  

Cloudbreak supports the following types of images for launching clusters:

| Image type | Description | Default images provided | Support for custom images |
|---|---|---|---|
| **Prewarmed Image** | By default, Cloudbreak launches clusters from prewarmed images. Prewarmed images include the operating system as well as Ambari and HDP/HDF. The Ambari and HDP/HDF version used by prewarmed images cannot be customized. | Yes | No |
| **Base Image** | Base images include default configuration and default tooling. These images include the operating system but do not include Ambari or HDP/HDF software. | Yes | Yes | 

By default, Cloudbreak uses the included default **prewarmed images**, which include the operating system, as well as
Ambari and HDP/HDF packages installed. You can optionally select the **base image** option if you would like to:

* Use an Ambari and HDP/HDF versions different than what the prewarmed image includes and/or  
* Choose a previously created custom base image

#### Choose image  

If under [Choose image catalog](#choose-image-catalog), you selected a custom image catalog, under **Choose Image** you can select an image from that catalog. For complete instructions, refer to [Using custom images](https://hortonworks.github.io/cloudbreak-documentation/latest/). 

If you are trying to customize Ambari and HDP/HDF versions, you can ignore the **Choose Image** option; in this case default base image is used.

####  Ambari repo specification

If you would like to use a custom Ambari version, provide the following information: 

| Parameter | Description | Example |
|---|---|---|
| Version | Enter Ambari version. | 2.6.1.3 |
| Repo Url | Provide a URL to the Ambari version repo that you would like to use. | http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.6.1.3 |
| Repo Gpg Key Url | Provide a URL to the repo GPG key. Each stable RPM package that is published by CentOS Project is signed with a GPG signature. By default, yum and the graphical update tools will verify these signatures and refuse to install any packages that are not signed, or have an incorrect signature. | http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins | 

#### HDP/HDF repo specification

If you would like to use a custom HDP or HDF version, provide the following information: 

| Parameter | Description | Example | 
|---|---|--|
| Stack | This is populated by default based on the "Platform Version" parameter. | HDP |
| Version | This is populated by default based on the "Platform Version" parameter. | 2.6 |
| OS | Operating system. | centos7 (Azure, GCP, OpenStack) or amazonlinux (AWS) |
| Repository Version | Enter repository version. | 2.6.4.0-91 |
| Version Definition File | Enter the URL of the VDF file. | http://public-repo-1.hortonworks.com/HDP/centos6/2.x/updates/2.6.4.0/HDP-2.6.4.0-91.xml |
| (HDF only) MPack Url | (HDF only) Provide MPack URL. | http://public-repo-1.hortonworks.com/HDF/centos7/3.x/updates/3.1.1.0/tars/hdf_ambari_mp/hdf-ambari-mpack-3.1.1.0-35.tar.gz |
| Enable Ambari Server to download and install GPL Licensed LZO packages? | (Optional, only available if using Ambari 2.6.1.0 or newer) Use this option to enable LZO compression in your HDP/HDF cluster. LZO is a lossless data compression library that favors speed over compression ratio. Ambari does not install nor enable LZO compression libraries by default, and must be explicitly configured to do so. For more information, refer to [Enabling LZO](https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-administration/content/enabling_lzo.html).| <img src="../images/cb_toggle.png" alt="On"/> |

If you choose to use a base image with custom Ambari and/or HDP/HDF version, Cloudbreak validates the information entered. When Cloudbreak detects that the information entered is incorrect, it displays a warning marked with the <img src="../images/cb_warning.png" width="25" title="Icon"> sign. You should review all the warnings before proceeding and make sure that the information that you entered is correct. If you choose to proceed in spite of the warnings, check "Ignore repository warnings".  
      


#### Recipes

This option allows you to select previously uploaded recipes (scripts that can be run pre or post cluster deployment) for each host group. For more information on recipes, refer to [Using custom scripts (recipes)](https://hortonworks.github.io/cloudbreak-documentation/latest/). 
 


#### Management packs

This option allows you to select previously uploaded management packs. For more information on management packs, refer to [Using management packs](https://hortonworks.github.io/cloudbreak-documentation/latest/). 
 


#### External sources 

You can register external sources with Cloudbreak, and then select and attach them during cluster create. To register external sources with Cloudbreak, refer to:

* [Using an external authentication source](https://hortonworks.github.io/cloudbreak-documentation/latest/)    
* [Using an external database](https://hortonworks.github.io/cloudbreak-documentation/latest/)  
* [Register a proxy](https://hortonworks.github.io/cloudbreak-documentation/latest/)  


#### Custom properties 

This option allows you to set custom properties based on the template defined in your custom blueprint. For more information, refer to [Set custom properties](https://hortonworks.github.io/cloudbreak-documentation/latest/). 
  


#### Single sign-on (SSO) 

This option allows you to configure the gateway to be the SSO identity provider. 

> This option is technical preview. 

For more information, refer to [Configuring the Gateway](https://hortonworks.github.io/cloudbreak-documentation/latest/) documentation. 


#### Ambari server master key 

The Ambari server master key is used to configure Ambari to encrypt database and Kerberos credentials that are retained by Ambari as part of the Ambari setup.  

[Comment]: <> (This is similar to "​Encrypt Database and LDAP Passwords documented at: https://docs.hortonworks.com/HDPDocuments/Ambari-2.6.1.0/bk_ambari-security/content/optional_encrypt_database_and_ldap_passwords.html)   


#### Enable Kerberos security 

Select this option to enable Kerberos for your cluster. For information about available Kerberos options, refer to [Enabling Kerberos security](https://hortonworks.github.io/cloudbreak-documentation/latest/). 

 


