
## Restrict inbound access to clusters 

We recommend that after launching Cloudbreak you set CB_DEFAULT_GATEWAY_CIDR in your Cloudbreak's Profile file.
When you launch a cluster, and Cloudbreak proposes security groups, this CIDR will be used
for the Cloudbreak to the Cluster master node (i.e. the host with Ambari Server) with this IP. This limits access
from Cloudbreak to this cluster name for ports 9443 and 22 for Cloudbreak communication and management of the cluster.

**Steps** 

1. Set CB_DEFAULT_GATEWAY_CIDR to the CIDR address range which is used by Cloudbreak to communicate with the cluster:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32</pre>
    
    Or, if your Cloudbreak communicates with the cluster through multiple addresses, set multiple addresses separated with a comma:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32,18.17.16.15/32</pre>
    
2. If Cloudbreak has already been started, restart it using `cbd restart`.     
    
3. When CB_DEFAULT_GATEWAY_CIDR is set, two additional rules are added to your Ambari node security group: (1) port 9443 open to your Cloudbreak IP, and (2) port 22 open to your Cloudbreak IP. You can view and edit these default rules in the create cluster wizard. 


