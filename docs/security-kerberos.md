## Enabling Kerberos Security

When creating a cluster, you can optionally enable Kerberos security in that cluster and provide your Kerberos configuration details. Cloudbreak will automatically extend your blueprint configuration with the defined properties.
	
### Kerberos Overview 

Kerberos is a third party authentication mechanism, in which users and services that users wish to access Hadoop rely on a third party - the Kerberos server - to authenticate each to the other. The Kerberos server itself is known as the **Key Distribution Center**, or **KDC**. At a high level, the KDC has three parts:

* A database of the users and services (known as **principals**) and their respective Kerberos passwords  
* An **Authentication Server (AS)** which performs the initial authentication and issues a Ticket Granting Ticket (TGT)  
* A **Ticket Granting Server (TGS)** that issues subsequent service tickets based on the initial TGT  

A user principal requests authentication from the AS. The AS returns a TGT that is encrypted using the user principal's Kerberos password, which is known only to the user principal and the AS. The user principal decrypts the TGT locally using its Kerberos password, and from that point forward, until the ticket expires, the user principal can use the TGT to get service tickets from the TGS. Service tickets are what allow the principal to access various services. 

Since cluster resources (hosts or services) cannot provide a password each time to decrypt the TGT, they use a special file, called a **keytab**, which contains the resource principal authentication credentials. The set of hosts, users, and services over which the Kerberos server has control is called a **realm**.

The following table explains the Kerberos related terminology:

| Term | Description |
|---|---|
| Key Distribution Center, or KDC | The trusted source for authentication in a Kerberos-enabled environment. |
| Kerberos KDC Server | The machine, or server, that serves as the Key Distribution Center (KDC). |
| Kerberos Client | Any machine in the cluster that authenticates against the KDC. |
| Principal | The unique name of a user or service that authenticates against the KDC. |
| Keytab | A file that includes one or more principals and their keys.
| Realm | The Kerberos network that includes a KDC and a number of clients. |

### Enabling Kerberos 

The option to enable Kerberos is available in the advanced **Security** section of the create cluster wizard.  

You have the following options for enabling Kerberos in a Cloudbreak  managed cluster:

| Option | Description | Environment |
|---|---|---|
| [Use Existing KDC](#use-existing-kdc) | <p>Allows you to leverage an existing MIT KDC or Active Directory for enabling Kerberos with the cluster.<p/><p>You can either provide the required parameters and Cloudbreak will generate the descriptors on your behalf, or provide the exact Ambari Kerberos descriptors to be injected into your blueprint in JSON format.</p> | Suitable for production |
| [Use Test KDC](#use-test-kdc) | <p>Installs a new MIT KDC on the master node and configures the cluster to leverage that KDC.</p> | Suitable for evaluation and testing only, not suitable for production |

#### Use Existing KDC 

To use an existing KDC, in the advanced **Security** section of the create cluster wizard select **Enable Kerberos Security**. By default, **Use Existing KDC** option is selected.  

You must provide the following information about your MIT KDC or Active Directory. Based on these parameters, kerberos-env and krb5-conf JSON descriptors for Ambari are generated and injected into your Blueprint:

> Before proceeding with the configuration, you must confirm that you met the requirements by checking the boxes next to all requirements listed. The configuration options are displayed only after you have confirmed all the requirements by checking every box.    

| Parameter | Description |
|---|---|
| Kerberos Admin Principal | The admin principal in your existing MIT KDC or AD. | 
| Kerberos Admin Password  | The admin principal password in your existing MIT KDC or AD. | 
| MIT KDC or Active Directory | Select MIT KDC or Active Directory. |

**Use Basic Configuration**

| Parameter | Required if using... | Description |
|---|---|---|
| Kerberos Url | MIT, AD | IP address or FQDN for the KDC host. Optionally a port number may be included. Example: "kdc.example.com:88" or "kdc.example.com" |
| Kerberos Admin URL | MIT, AD | (Optional) Description TBD. Example: TBD |
| Kerberos Realm | MIT, AD | The default realm to use when creating service principals. Example: "EXAMPLE.COM" |
| Kerberos AD Ldap Url | AD | The URL to the Active Directory LDAP Interface. This value must indicate a secure channel using LDAPS since it is required for creating and updating passwords for Active Directory accounts. Example: "ldaps://ad.example.com:636" |
| Kerberos AD Container DN | AD | The distinguished name (DN) of the container used store service principals. Example:  "OU=hadoop,DC=example,DC=com" |
| Use TCP Connection | Optional | By default, Kerberos uses UDP. Checkmark this box to use TCP instead. |

**Use Advanced Configuration** 

Checking the **Use Custom Configuration** option allows you to provide the actual Ambari Kerberos descriptors to be injected into your blueprint (instead of Cloudbreak generating the descriptors on your behalf). This is the most powerful option which gives you full control of the Ambari Kerberos options that are available. You must provide: 

* Kerberos-env JSON Descriptor (required)
* krb5-conf JSON Descriptor (optional)

To learn more about the Ambari Kerberos JSON descriptors, refer to [Apache cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Automated+Kerberizaton#AutomatedKerberizaton-Configurations).  


#### Use Test KDC 

To use a test KDC, in the advanced **Security** section of the create cluster wizard select **Enable Kerberos Security** and then select **Use Test KDC**.

<div class="danger">
<p class="first admonition-title">Important</p>
<p class="last">
Using the Test KDC is for evaluation and testing purposes only, and cannot be used for production clusters. To enable Kerberos for production use, you must use the <a href=""../security-kerberos/index.html#use-existing-kdc">Use Existing KDC</a> option. </p>
</div>
 
You must provide the following parameters for your new test KDC:  

| Parameter | Description |
|---|---|
| Kerberos Master Key | The master key for the KDC database. | 
| Kerberos Admin Username | The admin principal to create that can administer the KDC. |
| Kerberos Admin Password | The admin principal password. |
| Confirm Kerberos Admin Password | The admin principal password. |  

When using the test KDC option:
    
* Cloudbreak installs an MIT KDC instance on the Ambari server node.  
* Kerberos clients are installed on all cluster nodes, and the krb5.conf is configured to use the MIT KDC.  
* The cluster is configured for Kerberos to use the MIT KDC. Very basic Ambari KSON Kerberos descriptors are generated and used accordingly.


Example kerberos-env JSON descriptor file:

<pre>{
      "kerberos-env" : {
        "properties" : {
          "kdc_type" : "mit-kdc",
          "kdc_hosts" : "ip-10-0-121-81.ec2.internal",
          "realm" : "EC2.INTERNAL",
          "encryption_types" : "aes des3-cbc-sha1 rc4 des-cbc-md5",
          "ldap_url" : "",
          "admin_server_host" : "ip-10-0-121-81.ec2.internal",
          "container_dn" : ""
        }
      }
    }</pre> 
    
Example krb5-conf JSON  descriptor file: 

<pre> {
      "krb5-conf" : {
        "properties" : {
          "domains" : ".ec2.internal",
          "manage_krb5_conf" : "true"
        }
      }
    }</pre>  


**Related Links**   
[Apache cwiki](https://cwiki.apache.org/confluence/display/AMBARI/Automated+Kerberizaton#AutomatedKerberizaton-Configurations) 
