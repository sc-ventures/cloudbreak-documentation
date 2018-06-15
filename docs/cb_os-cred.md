### Create Cloudbreak credential

Cloudbreak works by connecting your OpenStack account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](https://hortonworks.github.io/cloudbreak-documentation/latest/).

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane.

2. Click **Create Credential**.

3. Under **Cloud provider**, select "Google Cloud Platform".

    <img src="../images/cb_cb-os-cred.png" width="650" title="Cloudbreak web UI">

3. Select the keystone version.

4. Provide the  following information:

    For Keystone v2:

    | Parameter | Description |
|---|---|
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| User | Enter your OpenStack user name. |
| Password | Enter your OpenStack password. |
| Tenant Name | Enter the OpenStack tenant name. |
| Endpoint | Enter the OpenStack endpoint. |
| API Facing | (Optional) Select *public*, *private*, or *internal*. |

    For Keystone v3:

    | Parameter | Description |
|---|---|
| Keystone scope | Select the scope: default, domain, or project. |
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| User | Enter your OpenStack user name. |
| Password | Enter your OpenStack password. |
| User Domain | Enter your OpenStack user domain. |
| Endpoint | Enter the OpenStack endpoint. |
| API Facing | (Optional) Select *public*, *private*, or *internal*. |

[comment]: <> (Not sure what these params do: Keystone scope, User Domain)

4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to [create clusters](https://hortonworks.github.io/cloudbreak-documentation/latest/).




