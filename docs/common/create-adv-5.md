
#### Security Groups 

For each host group, you have two options:

| Option | Description |
|---|---|
| User Defined | <p>(Default) Creates a new security group with the rules that you defined:</p><p><ul><li>A set of [default rules](security.md#default-cluster-security-groups) is provided. You should review and adjust these default rules. If you do not make any modifications, these default rules will be applied. </li><li>You may open ports by defining the CIDR, entering port range, selecting protocol and clicking **+**.</li><li>You may delete default or previously added rules using the delete icon.</li><ul></p> |  
| Provider Security Groups | Allows you to select an existing security group that is already available in the selected provider region. |  


**Related Links**   
[Default Cluster Security Groups](security.md#default-cluster-security-groups)


#### Enable Kerberos Security 

Select this option to enable Kerberos for your cluster. You will have an option to create a new kerberos or use an existing one. For more information refer to [Kerberos](security-kerberos.md) documentation. 

**Related Links**   
[Kerberos](security-kerberos.md)
