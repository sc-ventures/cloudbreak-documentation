
#### Security Groups 

For each host group, select one of the options:

* Create new security group  
* Do not use security group  
* Select an existing security group

If you choose to create a new security group, the *New Security Group* wizard will open.
    
1. TCP ports 22, 443, and 9443 will be open by default. These ports must be open on every security group; otherwise Cloudbreak will not be able to communicate with your provisioned cluster.  
2. You may open additional ports by defining the **CIDR**, **Port**, and **Protocol** for each and clicking **Add Rule**. 
3. Once done, click **SAVE** to save your security group settings.
4. Once you define a custom security group for one host group, you can reuse this definition for other node groups.


#### Enable Kerberos Security 

Select this option to enable Kerberos for your cluster. You will have an option to create a new kerberos or use an existing one. For more information refer to [Kerberos](security-kerberos.md) documentation. 

**Related Links**   
[Kerberos](security-kerberos.md)
