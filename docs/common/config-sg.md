| CIDR | Port | Protocol |
|---|---|---|
| 0.0.0.0/0 | 22 | tcp |
| 0.0.0.0/0 | 433 | tcp |
| 0.0.0.0/0 | 9443 | tcp |

No resources will be created until you create a cluster using this configuration.
  

#### Add Custom Security Group 

You can define reusable security group configurations for your clusters in the **manage security groups** tab: 

1. Click on **Create a new security group**.
2. Provide required parameters:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your security group configuration template. |
| Description | (Optional) Enter a description.|
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this security group template to create clusters, but they cannot delete it. | 

3. Provide the following parameters in order to define **Security Rules** for this security group:

    | Parameter | Value |
|---|---| 
| CIDR | Enter a valid [CIDR IP](http://www.ipaddressguide.com/cidr), from which the cluster will be accessed. |
| Port | Enter ports that you want to open. You can either list multiple ports, separated by a comma (for example "22,443,9443"), or you can define port ranges (for example "1-65355").|
| Protocol | Enter protocol that you want to use. |

    > Ports 22, 443, and 9443 must be open on every security group; otherwise Cloudbreak will not be able to communicate with your provisioned cluster.

4. Click **+Add Rule** to save the security rules. If needed, click **+Add Rule** again to display a new *Security Rules* form and add another set of rules. 

5. Click **+create security group** to save the configuration. 

    No resources will be created until you create a cluster using this configuration.


