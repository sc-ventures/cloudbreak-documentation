## Meet the prerequisites on OpenStack

Before launching Cloudbreak on OpenStack, review and meet the prerequisites. 

### Meet minimum system requirements

Before launching Cloudbreak on your OpenStack, make sure that your OpenStack deployment fulfills the following requirements.

#### Supported Linux distributions

The following versions of the [Red Hat distribution of OpenStack](https://www.rdoproject.org/) (RDO) are supported:

* Juno
* Kilo
* Liberty
* Mitaka

#### Standard modules

Cloudbreak requires that the following standard modules are installed and configured on OpenStack:

* Keystone V2 or Keystone V3
* Neutron (Self-service and provider networking)
* Nova (KVM or Xen hypervisor)
* Glance
* Cinder (Optional)
* Heat (Optional but highly recommended, since provisioning through native API calls will be deprecated in the future)

**Related links**  
[Red Hat distribution of OpenStack](https://www.rdoproject.org/) (External)


### Meet the prerequisites

Before launching Cloudbreak on OpenStack, you must meet the following prerequisites.

#### SSH key pair

[Generate a new SSH key pair](faq.md#generate-ssh-key-pair) or use an existing SSH key pair to your OpenStack account. You will be required to select it when launching the VM.

#### Security group

In order to launch Cloudbreak, you must have an existing security group with the following ports open: 22 (for access via SSH), 80 (for access via HTTP), and 443 (for access via HTTPS).

For information about OpenStack security groups, refer to the [OpenStack Administrator Guide](https://docs.openstack.org/ops-guide/index.html).

**Related links**  
[OpenStack Administrator Guide](https://docs.openstack.org/ops-guide/index.html) (External)


### Launch the VM

In your OpenStack, launch and instance providing the following parameters:

* Select a VM flavor which meets the following minimum requirements: 4GB RAM, 10GB disk, 2 cores.
* Select the Cloudbreak deployer image that you imported earlier and launch an instance using that image.
* Select your SSH key pair.
* Select the security group which has the following ports open: 22 (SSH) and 443 (HTTPS).
* Select your preconfigured network.


### SSH to the VM

Now that your VM is ready, access it via SSH:

* Use a private key matching the public key that you added to your OpenStack project.
* The SSH user is called "cloudbreak".
* You can obtain the VM's IP address from the details of your instance.

On Mac OS X, you can SSH to the VM by running the following from the Terminal app: `ssh -i "your-private-key.pem" cloudbreak@instance_IP` where "your-private-key.pem" points to the location of your private key and "instance_IP" is the public IP address of the VM.

On Windows, you can use [PuTTy](http://www.putty.org/).


<div class="next">
<a href="../os-launch/index.html">Next: Launch Cloudbreak</a>
</div>

