 
## Minimum System Requirements

Before launching Cloudbreak on your OpenStack, make sure that your OpenStack deployment fulfills the following requirements.

### Supported Linux Distributions

The following versions of the [Red Hat distribution of OpenStack](https://www.rdoproject.org/) (RDO) are supported:

* Juno
* Kilo
* Liberty
* Mitaka

### Standard Modules

Cloudbreak requires that the following standard modules are installed and configured on OpenStack:

* Keystone V2 or Keystone V3
* Neutron (Self-service and provider networking)
* Nova (KVM or Xen hypervisor)
* Glance
* Cinder (Optional)
* Heat (Optional but highly recommended, since provisioning through native API calls will be deprecated in the future)

**Related links**  
[Red Hat distribution of OpenStack](https://www.rdoproject.org/) (External)


## Prerequisites on OpenStack

Before launching Cloudbreak on OpenStack, you must meet the following prerequisites.

### SSH Key Pair

[Generate a new SSH key pair](faq.md#generate-ssh-key-pair) or use an existing SSH key pair to your OpenStack account. You will be required to select it when launching the VM.


{!docs/common/vm-pre.md!}


## Launching Cloudbreak on OpenStack

These steps describe how to launch Cloudbreak on OpenStack. This is the only deployment options available on OpenStack.  
Before launching Cloudbreak on OpenStack, review and meet the [prerequisites](#prerequisites). Next, follow the steps below. 

### Manually Import HDP and HDF Images to OpenStack

An OpenStack administrator can perform these steps to add the Cloudbreak deployer image to your OpenStack deployment. Perform these steps for each image. 

The following images can be imported:

| Image | Operating system | Location |
|---|---|---|
| Prewarmed HDP 2.6 image | centos7 | https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1807101522.img |
| Base image | centos7 | https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp--1801261636.img |

**Steps**

1. Download the image to your local machine. For example: 

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1807101522.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp-26-1807101522.img
export CB_LATEST_IMAGE_NAME=cb-hdp-26-1807101522.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see these image among your OpenStack images.



{!docs/common/vm-req.md!}

{!docs/common/vm-launch.md!} 


### Configure a Self-signed Certificate

If your OpenStack is secured with a self-signed certificate, you need to import that certificate into Cloudbreak, or else Cloudbreak won't be able to communicate with your OpenStack.

To import the certificate, place the certificate file in the `/certs/trusted/` directory, follow these steps.

**Steps**

1. Navigate to the `certs` directory (automatically generated).
2. Create the `trusted` directory.
3. Copy the certificate to the `trusted` directory.

Cloudbreak will automatically pick up the certificate and import it into its trust store upon start.


### Create Cloudbreak Credential

Cloudbreak works by connecting your OpenStack account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](concepts.md#cloudbreak-credential).

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



