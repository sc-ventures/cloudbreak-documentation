
## Network and Security

### Virtual Network

Azure uses Virtual network (VNet) service to create virtual networks that resembles a traditional networks. Your Cloudbreak controller and clusters are launched into the virtual network infrastructure, with a new VNet created for each resource (Cloudbreak controller, cluster1, cluster2, and so on).

### Network Security Groups

Network security groups are set up to control network traffic to the VMs in the system. By default, the system is configured to restrict inbound network traffic to the minimal set of ports. You can add or modify rules to each security group that allow traffic to or from its associated instances.

This section describes the default security group configuration for the various components in the system.

> The inbound and outbound rules (protocols, port and IP ranges) for the security groups can be modified later using the Network Security Groups dashboard.

The naming convention for the security groups that are automatically created is:

* Cloudbreak controller VM: cbdeployerNsg
* Cluster node VMs: {host_group_name}{cluster_name}sg

The following security groups are created automatically:

#### Cloudbreak Security Group

The **cbdeployerNsg** security group is created when launching  Cloudbreak and is associated with the Cloudbreak VM. The following table lists the security group port configuration for the cloud controller instance.
The security group *Source* for these ports is set to the **Remote Access** CIDR IP specified when
launching Cloudbreak.

| Inbound Port | Description |
|---|---|
| 22 | SSH access to the Cloudbreak VM. |
| 80 | HTTP access to the Cloudbreak UI. This is automatically redirected to the HTTPS (443) port. |
| 443 | HTTPS access to the Cloudbreak UI. |


#### Cluster Security Groups

Multiple security groups are created when you create a cluster, one for each host group. The
security group *Source* for these ports is set to the **Remote Access** CIDR IP specified when creating the cluster.

The following table lists the **master node** security group port configuration.

| Inbound Port | Description |
|---|---|
| 22 | SSH access to the node instance. |
| 9443 | Internal management port, used by the cloud controller to communicate with the cluster master node. |
| 8443<sup>1</sup> | Secured HTTPS gateway access to the Ambari, Zeppelin, Hive JDBC, and other Cluster Components. |

> <sup>1</sup> Port 8443 is only opened on the master node if when you create cluster, you check the checkbox under **Setup Network and Security > Enable Knox Gateway** (to Ambari and Zeppelin Web UIs, Hive JDBC and/or Cluster Components UIs).

The following table lists the security group port configuration for other host groups:

| Inbound Port | Description |
|---|---|
| 22 | SSH access to the node instance. |


<div class="note">
    <p class="first admonition-title">Learn More</p>
    <p class="last">
Refer to the Azure network security groups <a href="https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg" target="_blank"> documentation</a>
for more information about viewing and modifying network security group rules for VMs.
    </p>
</div>
