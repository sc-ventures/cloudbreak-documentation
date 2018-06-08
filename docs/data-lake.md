## Setting up a data lake

 > This feature is technical preview.  
 
A **data lake** provides a way for you to centrally apply and enforce authentication, authorization, and audit policies across multiple ephemeral workload clusters. "Attaching" your workload cluster to the data lake instance allows the attached cluster workloads to access data and run in the security context provided by the data lake. 

While workloads are temporary, the security policies around your data schema are long-running and shared for all workloads. As your workloads come and go, the data lake instance lives on, providing consistent and available security policy definitions that are available for current and future ephemeral workloads. All information related to schema (Hive), security policies (Ranger) and audit (Ranger) is stored on external locations (external databases and cloud storage).

Once you’ve created a data lake instance, you have an option to attach it to one or more ephemeral clusters. This allows you to apply the authentication, authorization, and audit across multiple workload clusters.

| Term | Description |
|---|---|
| Authentication source | User source for authentication and definition of groups for authorization. |
| Data lake | Runs Ranger, which is used for configuring authorization. policies and is used for audit capture. Runs Hive Metastore, which is used for data schema. |
| Attached clusters | The clusters that get attached to the data lake to run workloads. This is where you run workloads such as Hive via [JDBC](hive.md). |

The components of the data lake include:

<a href="../images/cb_datalake-diag01.png" target="_blank" title="click to enlarge"><img src="../images/cb_datalake-diag01.png" width="550" title="Cloudbreak architecture"></a>

| Component | Technology | Description |
|---|---|---|
| Schema | Apache Hive | Provides Hive schema (tables, views, and so on). If you have two or more workloads accessing the same Hive data, you need to share schema across these workloads. |
| Policy | Apache Ranger | Defines security policies around Hive schema. If you have two or more users accessing the same data, you need security policies to be consistently available and enforced. |
| Audit | Apache Ranger | Audits user access and captures data access activity for the workloads. |
| [Directory ](external-ldap.md)| LDAP/AD | Provides an authentication source for users and a definition of groups for authorization. |
| [Gateway](gateway.md) | Apache Knox | Supports a single workload endpoint that can be protected with SSL and enabled for authentication to access to resources. |

