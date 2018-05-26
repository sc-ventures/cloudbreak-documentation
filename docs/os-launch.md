## Launching Cloudbreak on OpenStack

Before launching Cloudbreak on OpenStack, review and meet the [prerequisites](os-pre.md). 


### Launch Cloudbreak on your own VM

Refer to [Launch Cloudbreak on your own VM](vm-launch.md). 


### Perform optional configurations

> These configurations are optional.


#### Configuring a self-signed certificate

If your OpenStack is secured with a self-signed certificate, you need to import that certificate into Cloudbreak, or else Cloudbreak won't be able to communicate with your OpenStack.

To import the certificate, place the certificate file in the `/certs/trusted/` directory, follow these steps.

**Steps**

1. Navigate to the `certs` directory (automatically generated).
2. Create the `trusted` directory.
3. Copy the certificate to the `trusted` directory.

Cloudbreak will automatically pick up the certificate and import it into its trust store upon start.


### Create Cloudbreak credential

Cloudbreak works by connecting your OpenStack account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a Cloudbreak credential.

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane.

2. Click **Create Credential**.

3. Under **Cloud provider**, select "Google Cloud Platform".

    <a href="../images/cb_cb-os-cred.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-os-cred.png" width="650" title="Cloudbreak web UI"></a>

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

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to [create clusters](os-create.md).

<div class="next">
<a href="../os-create/index.html">Next: Create a Cluster</a>
</div>


### Optional Steps 

#### Import images to OpenStack

An OpenStack administrator must perform these steps to add the Cloudbreak deployer image to your OpenStack deployment. 

> Importing prewarmed and base HDP and HDF images is no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDP cluster.

##### Import HDP prewarmed image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDP cluster. If you would like to import HDP prewarmed images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDP image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1803301748.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp-26-1803301748.img
export CB_LATEST_IMAGE_NAME=cb-hdp-26-1803301748.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images.


##### Import HDF prewarmed image

No prewarmed image is currently available for HDF. 


##### Import HDP/HDF base image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDP or HDF cluster. If you would like to import HDP/HDF base images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDP image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp--1803211203.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp--1803211203.img
export CB_LATEST_IMAGE_NAME=cb-hdp--1803211203.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images.

<div class="next">
<a href="../os-create/index.html">Next: Create a Cluster</a>
</div>

