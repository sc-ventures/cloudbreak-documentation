## Configuring an Authentication Source 


You can register an existing LDAP with Cloudbreak and use it for one or more clusters.

<div class="danger">
    <p class="first admonition-title">Important</p>
    <p class="last">
Registering an authentication source <strong>does not create the LDAP instance</strong>. You must have an existing LDAP/AD instance prior to registering it.
	</p>
</div> 

**Steps**

1. From the controller UI navigation menu, select **SHARED SERVICES**.

1. The list of registered shared services is displayed.

1. Click **+NEW** and select **Authentication Source**. The registration form is displayed.

1. Provide the following parameters related to your existing LDAP/AD: 

    > All parameters are required.
    
    **GENERAL CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| Name |  Enter a name for your data lake. | hdc-ldap |
| Directory Type | Choose whether your directory is **LDAP** or **Active Directory**. | LDAP |
| LDAP Server Connection | Select **LDAP** or **LDAPS**. Next, enter the hostname and port for the LDAP or AD server in the following format: `HOST_IP:PORT`. | `10.0.3.128:389` |
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
| LDAP Group Name Attribute | Enter the attribute for which to conduct a search on groups.  | `cn` |
| LDAP Group Object Class | Enter the directory object class for groups. | `group`  |
| LDAP Group Member Attribute | Enter the attribute on the group object class that represents members. | `member` |

5. Click **Test connection** to verify that the connection information that you entered is correct.
 
6. Click **REGISTER LDAP**. 




