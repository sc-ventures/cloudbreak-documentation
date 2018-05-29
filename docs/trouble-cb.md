## Troubleshooting Cloudbreak

This section includes common errors and steps to resolve them. 


### Invalid PUBLIC_IP in CBD Profile

The `PUBLIC_IP` property must be set in the cbd Profile file or else you won’t be able to log in on the Cloudbreak UI. 

If you are migrating your instance, make sure that after the start the IP remains valid. If you need to edit the `PUBLIC_IP` property in Profile, make sure to restart Cloudbreak using `cbd restart`.


### Cbd cannot get VM's public IP 

By default the `cbd` tool tries to get the VM's public IP to bind Cloudbreak UI to it. But if `cbd` cannot get the IP address during the initialization, you must set it manually. Check your `Profile` and if `PUBLIC_IP` is not set, add the `PUBLIC_IP` variable and set it to the public IP of the VM. For example: 

<pre>export PUBLIC_IP=192.134.23.10</pre>


### Permission or connection problems 

[comment]: <> (Not sure what this refers to. It came from the Install on Your Own VM docs.)

If you face permission or connection issues, disable SELinux:

1. Set `SELINUX=disabled` in `/etc/selinux/config`.  
2. Reboot the machine.  
3. Ensure the SELinux is not turned on afterwards:

    <presetenforce 0 && sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/ selinux/config</pre>
 





[Comment]: <> (More: https://drive.google.com/drive/u/0/folders/1Ml8hU3pgphYt47LLWHilRpGQEo6sNee3) 
