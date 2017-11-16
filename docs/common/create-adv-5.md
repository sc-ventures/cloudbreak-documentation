
#### Security Groups 

For each host group, select one of the following options:

* (Default) Create a new security group with [default rules](security.md#default-cluster-security-groups)    
* Create new security group with custom rules  
* Select an existing security group  
* (Only available for existing network and subnet) Do not use security group   

Unless you make a different selection, the cluster security groups will be created using the [default rules](security.md#default-cluster-security-groups): 

* *default-ambari-security-group*: for Ambari host group  
* *default-security-group*: for all other host groups     

<div class="danger">
    <p class="first admonition-title">Important</p>
    <p class="last">
By default, port 9443 is set to 0.0.0.0/0 CIDR for inbound access on the <b>default-ambari-security-group</b>. If you choose to use this default <b>default-ambari-security-group</b>  for your Ambari host group security group, It is strongly recommended that you limit this CIDR in the security group to only allow traffic from your Cloudbreak VM instance IP. 
</p>
</div>


If you choose to create a new security group, the *New Security Group* wizard will open.
    
1. You may open ports by defining the **CIDR**, **Port**, and **Protocol** for each and clicking **Add Rule**.   
2. Once done, click **SAVE** to save your security group settings.  
3. Once you define a custom security group for one host group, you can reuse this definition for other node groups.  

**Related Links**   
[Default Cluster Security Groups](security.md#default-cluster-security-groups)


#### Enable Kerberos Security 

Select this option to enable Kerberos for your cluster. You will have an option to create a new kerberos or use an existing one. For more information refer to [Kerberos](security-kerberos.md) documentation. 

**Related Links**   
[Kerberos](security-kerberos.md)
