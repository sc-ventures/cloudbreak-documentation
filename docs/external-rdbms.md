## Configuring External RDBMS 

When creating a cluster, you have an option to attach an external database that you have previously created. 

> You must create the external RDBMS instance and database prior to registering it with Cloudbreak.

**Steps** 

1. Create an instance of <a href="https://aws.amazon.com/rds/" target="_blank">Amazon RDS</a> with the **PostgreSQL DB Engine**.

    > Only PostgreSQL Engine is supported at this time.

2. From the controller UI navigation menu, select **SHARED SERVICES**.

3. The list of registered shared services is displayed.

4. Click **+NEW** and select **Hive Metastore** or **Druid Metastore**. The registration form is displayed.

5. Enter the following parameters:

    | Parameter | Description |
    |:---|:---|
    | Name | Enter the name to use when registering this metastore to the cloud controller. Allowed characters are lowercase letters, numbers, and dashes. This is **not** the database name. |
    | HDP Version | Select the version of HDP that this metastore can be used with. |
    | JDBC Connection | Select the database **type** (PostgreSQL) and enter the **JDBC connection** string (HOST:PORT/DB_NAME). |
    | Authentication | Enter the JDBC connection username and password. |

6. Click **Test connection** to validate and test the RDS connection information. If you experience connection issues, refer to the <a href="http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html" target="_blank">Amazon RDS Troubleshooting documentation</a>.

7. Once your settings are validated and working, click **REGISTER HIVE METASTORE** or **REGISTER DRUID METASTORE** to save the metastore. The metastore will
now show up in the list of available metastores when [creating a cluster](aws-create.md).


