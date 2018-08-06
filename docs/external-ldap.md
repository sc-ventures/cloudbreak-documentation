## Using an external authentication source for clusters

Cloudbreak allows you to register an existing LDAP/AD instance as an [external source](concepts.md#external-sources) and use it for multiple clusters. You must create the LDAP/AD prior to registering it with Cloudbreak. Once you have it ready, the overall steps are:
   
1. Register an existing LDAP in Cloudbreak web UI or CLI.  
1. Once registered, the LDAP will now show up in the list of available authentication sources when creating a cluster under advanced **External Sources** > **Configure Authentication**.  
1. Prepare a blueprint as described in [Preparing a blueprint for LDAP/AD ](#preparing-a-blueprint-for-ldapad).  
1. Create a cluster by using the blueprint and by attaching the authentication source. Cloudbreak automatically injects the LDAP property variables into the blueprint. 


### Preparing a blueprint for LDAP/AD 

In order to use LDAP/AD for your cluster, you must provide a suitable cluster blueprint:

- The blueprint must include one or more of the following supported components: Atlas, Hadoop, Hive LLAP, Ranger Admin, Ranger UserSync.  
- The blueprint should not include any LDAP properties. Before injecting the properties, Cloudbreak checks if LDAP related properties already exist in the blueprint. If they exist, they are not injected.  

During cluster creation the following properties will be injected in the blueprint:

- ldap.connectionURL  
- ldap.domain  
- ldap.bindDn  
- ldap.bindPassword  
- ldap.userSearchBase  
- ldap.userObjectClass  
- ldap.userNameAttribute  
- ldap.groupSearchBase  
- ldap.groupObjectClass  
- ldap.groupNameAttribute  
- ldap.groupMemberAttribute  
- ldap.directoryType  
- ldap.directoryTypeShort  

Their values will be the values that you provided to Cloudbreak: 

<a href="../images/cb_cb-ldap.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ldap.png" width="550" title="Cloudbreak web UI"></a>

[Source]: <> (Source https://docs.google.com/presentation/d/1vo3aZVMX0vx9gHZ5hALgxkGHw3l1PW-bGbxaYjk57Uo/edit#slide=id.p1)


### Register an authentication source 

Cloudbreak allows you to register an existing LDAP/AD instance and use it for multiple clusters. You must create the LDAP/AD prior to registering it with Cloudbreak. Once you have it ready, you can:

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
| Server Host | Enter the hostname or IP address for the LDAP or AD server. |`10.0.3.128`|
| Server Port | Enter the LDAP server port. | `389` |
| LDAP Domain | (Optional) Enter your LDAP domain if applicable. | `ad.mytestldap.com` |
| LDAP Bind DN | Enter the LDAP Bind DN | `CN=Administrator,CN=Users,DC=ad,DC=hdc,DC=com`   |
| LDAP Bind Password | Enter the LDAP Bind DN password.  | `MyPassword1234!` |

    **USER CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| LDAP User Search Base | Enter your LDAP user search base. This defines the location in the directory from which the LDAP search begins. | `CN=Users,DC=ad,DC=hdc,DC=com`  |
| LDAP User Name Attribute | Enter the attribute for which to conduct a search on the user base.  | `HDCaccountName` |
| LDAP User Dn Pattern | Enter LDAP User DN Pattern, which is used to bind an LDAP user.| `CN={0},CN=Users,DC=ad,DC=hdc,DC=com` |
| LDAP User Object Class | Enter the directory object class filter for users. | `person` |

    **GROUP CONFIGURATION**

    | Parameter | Description | Example |
|---|---|---|
| LDAP Group Search Base | Enter your LDAP group search base. This defines the location in the directory from which the LDAP search begins. | `CN=Users,DC=ad,DC=hdc,DC=com` |
| LDAP Admin Group | (Optional) Enter your LDAP admin group if applicable. |  |
| LDAP Group Name Attribute | Enter the attribute for which to conduct a search on groups.  | `cn` |
| LDAP Group Object Class | Enter the directory object class filter for groups. | `group`  |
| LDAP Group Member Attribute | Enter the attribute on the group object class that represents members. | `member` |

5. Click **Test Connection** to verify that the connection information that you entered is correct.
 
6. Click **REGISTER**. 

7. The LDAP will now show up on the list of available authentication sources when creating a cluster under advanced **External Sources** > **Configure Authentication**. It can be reused with multiple clusters. Just select it if you would like to use it for a given cluster.     
