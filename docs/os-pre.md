 
## Meet minimum system requirements

Before launching Cloudbreak on your OpenStack, make sure that your OpenStack deployment fulfills the following requirements.

### Supported Linux distributions

The following versions of the [Red Hat distribution of OpenStack](https://www.rdoproject.org/) (RDO) are supported:

* Juno
* Kilo
* Liberty
* Mitaka

### Standard modules

Cloudbreak requires that the following standard modules are installed and configured on OpenStack:

* Keystone V2 or Keystone V3
* Neutron (Self-service and provider networking)
* Nova (KVM or Xen hypervisor)
* Glance
* Cinder (Optional)
* Heat  

**Related links**  
[Red Hat distribution of OpenStack](https://www.rdoproject.org/) (External)


## Meet the prerequisites

Before launching Cloudbreak on OpenStack, you must meet the following prerequisites.

### SSH key pair

[Generate a new SSH key pair](faq.md#generate-ssh-key-pair) or use an existing SSH key pair to your OpenStack account. You will be required to select it when launching the VM.


{!docs/common/vm-pre.md!}




<div class="next">
<a href="../os-launch/index.html">Next: Launch Cloudbreak</a>
</div>

