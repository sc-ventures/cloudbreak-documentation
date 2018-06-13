## Creating HDF clusters 

In general, the create cluster wizard offers prescriptive default settings that help you configure your HDF clusters properly; however, there are a few additional configuration requirements that you should be aware of. 

### Creating HDF Flow Management clusters

When creating a Flow Management cluster from the default blueprint, make sure to do the following:

* On the **Hardware and Storage** page, place the **Ambari Server** on the "Services" host group. 
* On the **Network** page, open the required ports:        
    * Open **9091** TCP port on the NiFi host group. This port is used by **NiFi web UI**; Without it, you will be unable to access the NiFi web UI.   
    * Open **61443** TCP port on the Services host group. This port is used by **NiFi Registry**.       
* You should either use your **existing LDAP** or enable **Kerberos**.      
    * If using **LDAP**, you must first [register it as an external authentication source](external-ldap.md) in the Cloudbreak web UI.   
    * If using **Kerberos**, you can either use your own Kerberos (for production) or select for Cloudbreak to create a test KDC (for evaluation only).  
*  When creating a HDF cluster with LDAP, on the **Security** page:
    * You must must specify a **Cluster User** that is a valid user in the LDAP. This is a limitation with NiFi/NiFi Registry. Only one login provider can be configured for those components, and if an LDAP is supplied, then the login provider is set to LDAP; Consequently, this requires that the initial admin be in the given LDAP.  
    * While the passwords don't need to match between the cluster user and the same-named LDAP user, any other components that are used by that cluster user would require the password entered for the cluster user, not the same-named LDAP user.        
* When creating the NiFi Registry controller service in NiFi, the internal hostname must be used, `e.g. https://ip-1-2-3-4.us-west-2.compute.internal:61443`   
* Although Cloudbreak allows cluster scaling (including autoscaling), scaling is not supported by NiFi:       
    * **Downscaling NiFi clusters is not supported** - as it can result in data loss when a node is removed that has not yet processed all the data on that node.       
    * There is a known issue related to upscaling; It is listed in the [Known Issues](#known-issues). 
    

#### Troubleshooting HDF Flow Management cluster creation 

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
        

### Creating HDF Messaging Management clusters

When creating a Messaging Management cluster from the default blueprint, make sure to do the following:

* On the **Hardware and Storage** page, place the **Ambari Server** on the "Services" host group.  
* When creating a cluster, open **3000** TCP port on the Services host group for Grafana.    


