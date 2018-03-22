## Register an Authentication Source 

Cloudbreak allows you to register an existing LDAP/AD instance and use it for multiple clusters.  

You must create the LDAP/AD prior to registering it with Cloudbreak. Once you have it ready, you can:

1. Register an existing LDAP in Cloudbreak web UI or CLI.  
2. Use it as an authentication source for your clusters. Once registered, the LDAP will now show up in the list of available authentication sources when creating a cluster under advanced **External Sources** > **Configure Authentication**.   

**Steps**

1. From the navigation pane, select **External Sources** > **Authentication Configurations**.  
2. Select **Register Authentication Source**.     
3. Provide the following parameters related to your existing LDAP/AD: 
    
    **GENERAL CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| Name |  Enter a name for your LDAP. | cb-ldap |
| Directory Type | Choose whether your directory is **LDAP** or **Active Directory**. | LDAP |
| LDAP Server Connection | Select **LDAP** or **LDAPS**. | LDAP |
| Server Host | Enter the hostname for the LDAP or AD server . |`10.0.3.128`|
| Server Port | Enter the port. | `389` |
| LDAP Bind DN | Enter the root Distinguished Name to search in the directory for users. | `CN=Administrator,CN=Users,DC=ad,DC=hdc,DC=com`   |
| LDAP Bind Password | Enter your root Distinguished Name password.  | `MyPassword1234!` |

    **USER CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| LDAP User Search Base | Enter your LDAP user search base. This defines the location in the directory from which the LDAP search begins. | `CN=Users,DC=ad,DC=hdc,DC=com`  |
| LDAP User Name Attribute | Enter the attribute for which to conduct a search on the user base.  | `HDCaccountName` |
| LDAP User Object Class | Enter the directory object class for users. | `person` |

    **GROUP CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| LDAP Group Search Base | Enter your LDAP group search base. This defines the location in the directory from which the LDAP search begins. | `OU-scoDC=ad,DC=hdc,DC=com`  |
| LDAP Admin Group | (Optional) Enter your LDAP admin group, if needed. |  |
| LDAP Group Name Attribute | Enter the attribute for which to conduct a search on groups.  | `cn` |
| LDAP Group Object Class | Enter the directory object class for groups. | `group`  |
| LDAP Group Member Attribute | Enter the attribute on the group object class that represents members. | `member` |

5. Click **Test Connection** to verify that the connection information that you entered is correct.
 
6. Click **REGISTER**. 

7. The LDAP will now show up on the list of available authentication sources when creating a cluster under advanced **External Sources** > **Configure Authentication**. It can be reused with multiple clusters. Just select it if you would like to use it for a given cluster.     





