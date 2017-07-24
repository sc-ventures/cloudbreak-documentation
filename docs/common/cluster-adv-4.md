
#### Choose Failure Action

You can optionally select what to do if cluster creation fails or if there aren't enough instances available to create all requested nodes:

| Parameter | Description |
|---|---|
| Failure Action | <p>Select one of: **do NOT rollback resources** (default) or **rollback resources**. </p><p>By default, if creating a cluster fails, the Azure resources that were created up to that point will not be rolled back. This means that they will remain accessible for troubleshooting and you will need to to delete them manually.</p> |
| Minimum Cluster Size | This defines the provisioning strategy in case the cloud provider cannot allocate all the requested nodes. Select **best effort** or **exact**.  |


#### Configure Ambari Repos

You can optionally configure a different version of Ambari than the default by providing the following information:

| Parameter | Description |
|---|---|
| Ambari Version| Enter Ambari version. |
| Ambari Repo URL | Enter Ambari repo URL. |
| Ambari Repo Gpg Key URL | Enter gpgkey URL. |


#### Configure HDP Repos

You can optionally configure a different version of HDP than the default by providing the following information:

| Parameter | Description |
|---|---|
| Stack | Enter stack name. |
| Version | Enter stack version. |
| Stack Repo ID | Enter stack repo ID. |
| Base URL| Ener stack repo base URL. |
| Utils Repo ID | Enter Utils repo ID. |
| Utils Base URL | Enter Utils repo base URL. |
| Verify | Select to verify the repo information. |


#### Configure Ambari Database

By default, Ambari stores data on an embedded database, which is sufficient for ephemeral or test clusters. However, as Ambari and Cloudbreak don't perform backups of this database, it is insufficient for long-running production clusters, and you may need to configure a remote database for Ambari and Cloudbreak.

| Parameter | Description |
|---|---|
| Vendor | Select database vendor from the list. |
| Host | Enter database host IP. |
| Port | Enter port number. |
| Name | Enter database name. |
| User Name | Enter database user name. |
| Password | Enter database password. |
