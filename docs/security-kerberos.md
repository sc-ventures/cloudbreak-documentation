## Configuring Kerberos

> This feature is `TECHNICAL PREVIEW`.

Cloudbreak supports using Kerberos security for its clusters. It supports three ways of provisioning Kerberos-enabled clusters:

* Create new MIT Kerberos at cluster provisioning time  
* Use your existing MIT Kerberos server with a Cloudbreak provisioned cluster  
* Use your existing Active Directory with a Cloudbreak provisioned cluster  

### Create New MIT Kerberos at Cluster Provisioning

When creating a cluster, you can provide your Kerberos configuration details and Cloudbreak will install an MIT KDC and enable Kerberos on the cluster.

To enable Kerberos on a cluster, follow these steps when creating your cluster via Cloudbreak web UI.

**Steps**

1. In the **Create cluster** wizard, in the **Setup Network and Security** tab, check the **Enable security** option and select **Create New MIT Kerberos**.

2. Provide the following information for the KDC:

| Field | Description |
|---|---|
| Kerberos master key | The master key to use for the KDC. |
| Kerberos admin | The KDC admin username to use for the KDC. |
| Kerberos password | The KDC admin password to use for the KDC. |


#### Testing Kerberos

To run a job on the cluster, you can use one of the default Hadoop users, like `ambari-qa`.

Once kerberos is enabled, you need a `ticket` to execute any job on the cluster. Here's an example of how to get a ticket:

```
kinit -V -kt /etc/security/keytabs/smokeuser.headless.keytab ambari-qa-sparktest-rec@NODE.DC1.CONSUL
```

Here is an example job:
```java
export HADOOP_LIBS=/usr/hdp/current/hadoop-mapreduce-client
export JAR_EXAMPLES=$HADOOP_LIBS/hadoop-mapreduce-examples.jar
export JAR_JOBCLIENT=$HADOOP_LIBS/hadoop-mapreduce-client-jobclient.jar

hadoop jar $JAR_EXAMPLES teragen 10000000 /user/ambari-qa/terasort-input

hadoop jar $JAR_JOBCLIENT mrbench -baseDir /user/ambari-qa/smallJobsBenchmark -numRuns 5 -maps 10 -reduces 5 -inputLines 10 -inputType ascending
```

### Use Existing MIT Kerberos Server with a Cloudbreak Provisioned Cluster

Cloudbreak supports using Kerberos security on the cluster with an existing MIT Kerberos. When creating a cluster, provide your Kerberos configuration details, and Cloudbreak will automatically extend your blueprint configuration with the defined properties. [Setup an exiting MIT KDC](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/_use_an_exisiting_mit_kdc.html)

To enable Kerberos on a cluster, perform the following steps when creating your cluster via Cloudbreak web UI.

**Steps**

1. In the **Create cluster** wizard, in the **Setup Network and Security** tab, check the **Enable security** option and select **Use Existing MIT Kerberos**.
2. Fill in the following fields:

| Field | Description |
|---|---|
| Kerberos Password | The KDC admin password to use for the KDC. |
| Existing Kerberos Principal | The KDC principal in your existing MIT KDC. |
| Existing Kerberos URL | The location of your existing MIT KDC. |
| Use Tcp Connection | The connection type for your existing MIT KDC (default is **UDP**). |
| Existing Kerberos Realm | The realm in your existing MIT KDC. |

To enable Kerberos on a cluster, do the following when creating your cluster via Cloudbreak shell:

| Parameter | Description |
|---|---|
| --kerberosPassword | The KDC admin password to use for the KDC. |
| --kerberosPrincipal | The KDC principal in your existing MIT KDC. |
| --kerberosUrl | The location of your existing MIT KDC. |
| --kerberosTcpAllowed | The connection type for your existing MIT KDC (default is **UDP**). |
| --kerberosRealm | The realm in your existing MIT KDC. |

### Use Existing Active Directory with a Cloudbreak Provisioned Cluster

Cloudbreak supports using Kerberos security on the cluster with an existing Active Directory. When creating a cluster, provide your Kerberos configuration details, and Cloudbreak will automatically extend your blueprint configuration with the defined properties. [Setup an Active Directory for Kerberos](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/_use_an_existing_active_directory_domain.html)

To enable Kerberos on a cluster, perform these steps when creating your cluster via Cloudbreak UI.

**Steps**

1. In the **Create cluster** wizard, in the **Setup Network and Security** tab, check the **Enable security** option and select **Use Existing Active Directory**.
2. Provide the following informstion:

| Field | Description |
|---|---|
| Kerberos Password | The KDC admin password to use for the KDC. |
| Existing Kerberos Principal | The KDC principal in your existing MIT KDC. |
| Existing Kerberos URL | The location of your existing MIT KDC. |
| Use Tcp Connection | The connection type for your existing MIT KDC (default is **UDP**). |
| Existing Kerberos Realm | The realm in your existing MIT KDC. |
| Existing Kerberos Ldap AD Url | The url of the existing secure ldap (eg. ldaps://10.1.1.5). |
| Existing Kerberos AD Container DN | Active Directory User container for principals. For example "OU=Hadoop,OU=People,dc=apache,dc=org". |


### Create Hadoop Users

To create Hadoop users, follow these steps.

**Steps**

* Log in via SSH to the node where the Ambari Server is (IP address is the same as the Ambari UI) and run:

    <pre>
kadmin -p [admin_user]/[admin_user]@NODE.DC1.CONSUL (type admin password)
addprinc custom-user (type user password twice)
</pre>

* Log in via SSH to all other nodes and, on each node, run:

    <pre>
useradd custom-user
</pre>

* Log in via SSH to one of the nodes and run:

    <pre>
su custom-user
kinit -p custom-user (type user password)
hdfs dfs -mkdir input
hdfs dfs -put /tmp/wait-for-host-number.sh input
yarn jar $(find /usr/hdp -name hadoop-mapreduce-examples.jar) wordcount input output
hdfs dfs -cat output/*
</pre>
