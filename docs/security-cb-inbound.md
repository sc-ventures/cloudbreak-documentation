
## Restrict Inbound Access to Clusters 

We recommend that after launching Cloudbreak you set  CB_DEFAULT_GATEWAY_CIDR in your Cloudbreak's Profile file in order to automatically open ports 9443 and 22 to your Cloudbreak IP on your Ambari node security group. 

**Steps** 

1. Set CB_DEFAULT_GATEWAY_CIDR to the CIDR address range which is used by Cloudbreak to communicate with the cluster:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32</pre>
    
    Or, if your Cloudbreak communicates with the cluster through multiple addresses, set multiple addresses separated with a comma:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32,18.17.16.15/32</pre>
    
2. If Cloudbreak has already been started, restart it using `cbd restart`.     
    
3. When CB_DEFAULT_GATEWAY_CIDR is set, two additional rules are added to your Ambari node security group: (1) port 9443 open to your Cloudbreak IP, and (2) port 22 open to your Cloudbrak IP. You can view and edit these default rules in the create cluster wizard. 


