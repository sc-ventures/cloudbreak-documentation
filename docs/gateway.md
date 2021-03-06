## Configuring the gateway

When creating a cluster, Cloudbreak installs and configures a gateway, powered by [Apache Knox](https://knox.apache.org/), to protect access to the cluster resources:

* This gateway is installed on the same host as the Ambari server.  
* By default, transport layer security on the gateway endpoint is via a *self-signed SSL certificate* on port 8443. This is illustrated on the following diagram:   
    
    <a href="../images/cb_gateway.png" target="_blank" title="click to enlarge"><img src="../images/cb_gateway.png" width="650" title="Cluster Information"></a> 

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

<sup>*</sup> Refer to [Accessing Hive via JDBC](hive.md#accessign-hive-via-jdbc) for more information.

The following applies:

* The `gateway-host` is the IP address or hostname of the Ambari server node.  
* The `cluster-name` is the name of the cluster.  
* The `topology-name` is the name of the gateway topology that you entered when creating the cluster. By default this is set to `db-proxy`.    


### Configure the gateway 

When creating a cluster, you can configure the gateway on the **Gateway Configuration** page of the basic create cluster wizard. 

**Steps**

1. In the create cluster wizard, navigate to the **Gateway Configuration** page: 

    <a href="../images/cb_cb-gateway01.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-gateway01.png" width="650" title="Cluster Information"></a>  
    
    
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

<a href="../images/cb_cb-gateway02.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-gateway02.png" width="650" title="Cluster Information"></a>

The URL structure is as described in [Services available via gateway](#services-available-via-gateway).


### Configure single sign-on (SSO) 

When creating a cluster, if you selected to configure a gateway, on the advanced **Gateway Configuration** page of the advanced create cluster wizard, you can also configure the gateway to be the SSO identity provider. 

> This option is technical preview. 

**Prerequisites**

You must have an existing authentication source (LDAP or AD) and register it with Cloudbreak, as described in [Using an external authentication source for clusters](external-ldap.md). 

**Steps**

1. In the create cluster wizard, select the advanced mode.  
1. On the **External Sources** page, under **Configure Authentication**, select to attach a previously configured LDAP to the cluster.  
2. On the **Gateway Configuration** page, under **Single Sign On (SSO)**, click the toggle button to enable SSO.

