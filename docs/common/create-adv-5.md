
#### Security Groups 

For each host group, select one of the options:

* Create new security group with custom rules  
* Create a new security group with [default rules](security.md#default-cluster-security-groups) 
* Select an existing security group  
* Do not use security group (only available for existing network and subnet)  
 

If you choose to create a new security group, the *New Security Group* wizard will open.
    
1. You may open ports by defining the **CIDR**, **Port**, and **Protocol** for each and clicking **Add Rule**.   
2. Once done, click **SAVE** to save your security group settings.  
3. Once you define a custom security group for one host group, you can reuse this definition for other node groups.  


#### Enable Kerberos Security 

Select this option to enable Kerberos for your cluster. You will have an option to create a new kerberos or use an existing one. For more information refer to [Kerberos](security-kerberos.md) documentation. 

**Related Links**   
[Kerberos](security-kerberos.md)
