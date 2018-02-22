## Securing Cloudbreak After Launch

Cloudbreak comes with default settings designed for easy first experience rather than strict security. To secure Cloudbreak, follow these recommendations. 

### Restricting Inbound Access to Cloudbreak  

We recommend that you block all communication ports except 22, 80, and 443 on the Cloudbreak's security group. If you have to log in to the Cloudbreak host remotely, use the SSH port (usually 22).


### Restricting Inbound Access to Clusters 

We recommend that after launching Cloudbreak you set  CB_DEFAULT_GATEWAY_CIDR in your Cloudbreak's Profile file in order to automatically open ports 9443 and 22 to your Cloudbreak IP on your Ambari node security group. 

**Steps** 

1. Set CB_DEFAULT_GATEWAY_CIDR to the CIDR address range which is used by Cloudbreak to communicate with the cluster:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32</pre>
    
    Or, if your Cloudbreak communicates with the cluster through multiple addresses, set multiple addresses separated with a comma:
    
    <pre>export CB_DEFAULT_GATEWAY_CIDR=14.15.16.17/32,18.17.16.15/32</pre>
    
2. If Cloudbreak has already been started, restart it using `cbd restart`.     
    
3. When CB_DEFAULT_GATEWAY_CIDR is set, two additional rules are added to your Ambari node security group: (1) port 9443 open to your Cloudbreak IP, and (2) port 22 open to your Cloudbrak IP. You can view and edit these default rules in the create cluster wizard. 


### Secure the Profile 

Before starting Cloudbreak for the first time, configure the Profile file as directed below. Changes are applied during startup so a restart (`cbd restart`) is required after each change.

1. Execute the following command in the directory where you want to store Cloudbreak-related files:

    <pre>
echo export PUBLIC_IP=[the ip or hostname to bind] > Profile
</pre>

[comment]: <> (TO-DO: Do you mean that this needs to be executed in the deployment directory? Or?)

2. After you have a base Profile file, add the following custom properties to it:

    <pre>
export UAA_DEFAULT_SECRET='[custom secret]'
export UAA_DEFAULT_USER_EMAIL='[default admin email address]'
export UAA_DEFAULT_USER_PW='[default admin password]'
export UAA_DEFAULT_USER_FIRSTNAME='[default admin first name]'
export UAA_DEFAULT_USER_LASTNAME='[default admin last name]'
</pre>

    Cloudbreak has additional secrets which by default inherit their values from `UAA_DEFAULT_SECRET`. Instead of using the default, you can define different values in the Profile for each of these service clients:

    <pre>
export UAA_CLOUDBREAK_SECRET='[cloudbreak secret]'
export UAA_PERISCOPE_SECRET='[auto scaling secret]'
export UAA_ULUWATU_SECRET='[web ui secret]'
export UAA_SULTANS_SECRET='[authenticator secret]'
</pre>

    You can change these secrets at any time, except `UAA_CLOUDBREAK_SECRET` which is used to encrypt sensitive information at database level. 
    
[comment]: <> (TO-DO: The info below is explained in a way that is confusing. Can you rephrase?) 

    `UAA_DEFAULT_USER_PW` is stored in plain text format, but if `UAA_DEFAULT_USER_PW` is missing from the Profile, it gets a default value. Because default password is not an option, if you set an empty password explicitly in the Profile Cloudbreak deployer will ask for password all the time when it is needed for the operation.

    <pre>
export UAA_DEFAULT_USER_PW=''
</pre>

    In this case, Cloudbreak deployer wouldn't be able to add the default user, so you have to do it manually by executing the following command:

    <pre>
cbd util add-default-user
</pre>

For more information about setting environment variables in Profile, refer to [Configure Profile Variables](cb-profile.md#configure-profile-variables).


### Add SSL Certificate for Cloudbreak UI 

By default Cloudbreak has been configured with a self-signed certificate for access via HTTPS. This is sufficient for many deployments such as trials, development, testing, or staging. However, for production deployments, a trusted certificate is preferred and can be configured in the controller. Follow these steps to configure the cloud controller to use your own trusted certificate. 

**Prerequisites**

To use your own certificate, you must have:

* A resolvable fully qualified domain name (FQDN) for the controller host IP address. For example, this can be set up in [Amazon Route 53](https://aws.amazon.com/documentation/route53/).  
* A valid SSL certificate for this fully qualified domain name. The certificate can be obtained from a number of certificate providers.  

**Steps**

1. SSH to the Cloudbreak host instance:

    <pre><small>ssh -i mykeypair.pem cloudbreak@[CONTROLLER-IP-ADDRESS]</small></pre>
    
2. Make sure that the target fully qualified domain name (FQDN) which you plan to use for Cloudbreak is resolvable:

    <pre><small>nslookup [TARGET-CONTROLLER-FQDN]</small></pre>
    
    For example:

    <pre><small>nslookup hdcloud.example.com</small></pre>
    
3. Browse to the Cloudbreak deployment directory and edit the `Profile` file:

    <pre><small>vi /var/lib/cloudbreak-deployment/Profile</small></pre>
    
4. Replace the value of the `PUBLIC_IP` variable with the `TARGET-CONTROLLER-FQDN` value:

    <pre><small>PUBLIC_IP=[TARGET-CONTROLLER-FQDN]</small></pre>
    
5. Copy your private key and certificate files for the FQDN onto the Cloudbreak host. These files must be placed under `/var/lib/cloudbreak-deployment/certs/traefik/` directory.

    > File permissions for the private key and certificate files can be set to 600.

    | File | Example |
|---|---|
| PRIV-KEY-LOCATION	| /var/lib/cloudbreak-deployment/certs/traefik/hdcloud.example.com.key |
| CERT-LOCATION	| /var/lib/cloudbreak-deployment/certs/traefik/hdcloud.example.com.crt |

6. Configure TLS details in your `Profile` by adding the following line at the end of the file.

    > Notice that `CERT-LOCATION` and `PRIV-KEY-LOCATION` are file locations from Step 5, starting at the `/certs/...` path.

    <pre><small>export CBD_TRAEFIK_TLS=”[CERT-LOCATION],[PRIV-KEY-LOCATION]”</small></pre>
    
    For example:

    <pre><small>export CBD_TRAEFIK_TLS="/certs/traefik/hdcloud.example.com.crt,/certs/traefik/hdcloud.example.com.key"</small></pre>
    
7. Restart Cloudbreak deployer:

    <pre><small>cbd restart</small></pre>
    
8. Using your web browser, access the Cloudbreak UI using the new resolvable fully qualified domain name.

9. Confirm that the connection is SSL-protected and that the certificate used is the certificate that you provided to Cloudbreak.


### Configure Access from Custom Domains

Cloudbreak deployer, which uses UAA as an identity provider, supports multitenancy. In UAA, multitenancy is managed through identity zones. An identity zone is accessed through a unique subdomain. For example, if the standard UAA responds to `https://uaa.10.244.0.34.xip.io`, a zone on this UAA can be accessed through a unique subdomain `https://testzone1.uaa.10.244.0.34.xip.io`.

If you want to use a custom domain for your identity or deployment, add the `UAA_ZONE_DOMAIN` line to your `Profile`:

<pre>export UAA_ZONE_DOMAIN=my-subdomain.example.com</pre>

This variable is necessary for UAA to identify which zone provider should handle the requests that arrive to that domain.


