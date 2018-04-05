## Configure Outbound Internet Access and Proxy  

Depending on your enterprise requirements, you may have limited or restricted outbound network access and/or require the use of an internet proxy. Installing and configuring Cloudbreak, as well as creating cloud resources and clusters on those resources requires outbound network access to certain destinations, and in some cases must go through a proxy.

This section provides information on the outbound network destinations for Cloudbreak, and instructions on how to configure Cloudbreak to use a proxy for outbound access (if required).

| Scenario | Documentation |
|---|---|
| My environment has limited outbound internet access | Refer to [Outbound Network Access Destinations](#outbound-network-access-destinations) for information on network rules. |
| My environment requires use of a proxy for outbound internet access | Refer to [Using a Proxy](#using-a-proxy) for information on using a proxy with Cloudbreak. | 


### Outbound Network Access Destinations

To install and configure Cloudbreak, you will need the following outbound destinations available:

<table>
<tr>
    <th>Destination</th>
    <th>Description</th> 
  </tr>
  <tr>
    <td>*.docker.io</td>
    <td>Obtain the Docker images for Cloudbreak.</td> 
  </tr>
  <tr>
    <td><p>raw.githubusercontent.com</p><p>github.com</p><p>s3.amazonaws.com</p><p>*.cloudfront.net</p></td>
    <td>Obtain Cloudbreak dependencies.</td> 
  </tr>
  <tr>
  <td>cloudbreak-imagecatalog.s3.amazonaws.com </td><td> The default Cloudbreak image catalog used for VMs. Refer to <a href="../images/index.html">Custom Images</a> for more information on image catalogs. </td>
  </tr>
</table>

Once Cloudbreak is installed and configured, you will need the following outbound destinations available in order to communicate with the cloud provider APIs to obtain cloud resources for clusters.

<table>
<tr>
    <th>Cloud Provider</th>
    <th>Cloud Provider API Destinations</th> 
  </tr>
  <tr>
  <td>Amazon Web Services</td>
  <td>*.amazonaws.com</td>
 </tr>
  <td> Microsoft Azure </td>
  <td> <p>*.microsoftonline.com</p><p>*.windows.net</p><p>*.azure.com</p></td>
   </tr>
   <tr>
  <td> Google Cloud Platform  </td>  
  <td> <p>accounts.google.com</p><p>*.googleapis.com</p></td>
  </tr>
</table>

To install the cluster software, you can: 

a) use the public hosted repositories provided by Hortonworks, or  
b) specify your own local hosted repositories when you create a cluster. 

If you choose to (a) use the public hosted repositories, be sure to allow outbound access to the following destinations:

* private-repo-1.hortonworks.com  
* public-repo-1.hortonworks.com  

### Using a Proxy 

In some cases, your environment requires all internet traffic to go through an internet proxy. This section describes the following:

* How to [set up Cloudbreak to use a proxy](#set-up-cloudbreak-to-use-a-proxy)  
* How to [configure your cluster hosts to use a proxy](#set-up-clusters-to-use-a-proxy)  

#### Set up Cloudbreak to Use a Proxy 

Use these steps if you would like to set up Cloudbreak to use your proxy. 

**Steps**

1. After downloading and installing Cloudbreak, configure the Docker daemon to use proxy by adding the following to the Docker service file:

    <pre>Environment="HTTP_PROXY=http://my-proxy-host:my-proxy-port" "NO_PROXY=localhost,127.0.0.1"</pre>   
    
    For example:
    
    <pre>vi /etc/systemd/system/docker.service -> Environment="HTTP_PROXY=http://10.0.2.237:3128" "NO_PROXY=localhost,127.0.0.1"</pre>

    For more information refer to
[Docker docs](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy).     

2. Ensure that ports 9443 and 8443 are handled as SSL connections in the proxy config.   

3. Configure proxy settings in the Profile file by setting the following variables:  

<pre>HTTP_PROXY_HOST=your-proxy-host
HTTPS_PROXY_HOST=your-proxy-host
PROXY_PORT=your-proxy-port
PROXY_USER=your-proxy-user
PROXY_PASSWORD=your-proxy-password
#NON_PROXY_HOSTS
#HTTPS_PROXYFORCLUSTERCONNECTION=false</pre>

For example:

<pre>HTTP_PROXY_HOST=10.0.2.237
HTTPS_PROXY_HOST=10.0.2.237
PROXY_PORT=3128
PROXY_USER=squid
PROXY_PASSWORD=squid
#NON_PROXY_HOSTS
#HTTPS_PROXYFORCLUSTERCONNECTION=false</pre>


#### Set up Clusters to Use a Proxy 

Use the following guidelines to find out what steps to perform in order to set up your clusters to use a proxy:  

| What base image are you using?| Where are the platform repositories? | What to do | 
|---|---|---|
| Default | Public | Use [Register a Proxy](external-proxy.md) |
| Default | Local | Use [Register a Proxy](external-proxy.md)|
| Custom | Public | Set up the proxy on your custom image OR use [Register a Proxy](external-proxy.md).|
| Custom | Local | Not required. Skip this section. |

You can define a proxy configuration as an external source in Cloudbreak web UI or CLI, and then (optionally) specify to configure that proxy configuration on the hosts that are part of the cluster during cluster create. Refer to [Register a Proxy](external-proxy.md) for more information.  


#### Advanced Proxy Setup Scenarios 

In some cases, Cloudbreak using the proxy might vary depending on your Cloudbreak -> cluster deployment. This section describes two scenarios:

* **Scenario 1**: Cloudbreak needs to go through a proxy to access the Cloud provider APIs (and other public internet resources) but can talk to the cluster hosts directly.  
* **Scenario 2**: Cloudbreak needs to go through a proxy to access the Cloud provider APIs (and other public internet resources) and the cluster hosts.  


##### **Scenario 1**

In this scenario, Cloudbreak can resolve and communicate with the Ambari Server in the cluster hosts directly. For example, this can be a scenario where Cloudbreak is deployed in the same VPC/VNet as the clusters and will not go through the proxy. However, Cloudbreak will communicate to the public Cloud Provider APIs via the proxy.

To configure this scenario, set this setting in your Profile file:

<pre>HTTPS_PROXYFORCLUSTERCONNECTION = false</pre>


<a href="../images/cb-proxy1.png" target="_blank" title="click to enlarge"><img src="../images/cb-proxy1.png" width="650" title="GCP Console"></a> 

##### **Scenario 2**

In this scenario, Cloudbreak will connect to the Ambari Server through the configured proxy. For example, this can be a scenario where Cloudbreak is deployed to a different VPC/VNet than the cluster and must go through a proxy. Communication to the public cloud provider APIs also is via the proxy.

To configure this scenario, set this setting in your Profile file:

<pre>HTTPS_PROXYFORCLUSTERCONNECTION = true</pre>

<a href="../images/cb-proxy2.png" target="_blank" title="click to enlarge"><img src="../images/cb-proxy2.png" width="650" title="GCP Console"></a> 


