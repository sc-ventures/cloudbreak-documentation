## Securing Cloudbreak After Launch

Cloudbreak comes with default settings designed for easy first experience rather than strict security. To secure Cloudbreak, follow these recommendations. 

### Restricting Inbound Access 

We recommend that you block all communication ports except 443 on the firewall or security group (depending on the provider). 

If you have to log in to the Cloudbreak host remotely, use the SSH port (usually 22).

>>>>TO-DO: Is the step above up-to-date? I think ports 80 and 22 is also open. 

### Configuring the Profile 

Before starting Cloudbreak for the first time, configure the Profile file as directed below. Changes are applied during startup so a restart (`cbd restart`) is required after each change.

1. Execute the following command in the directory where you want to store Cloudbreak-related files:

    <pre>
echo export PUBLIC_IP=[the ip or hostname to bind] > Profile
</pre>

    >>>>TO-DO: Do you mean that this needs to be executed in the deployment directory? Or?

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
    
    >>>>TO-DO: The info below is explained in a way that is confusing. Can you rephrase? 

    `UAA_DEFAULT_USER_PW` is stored in plain text format, but if `UAA_DEFAULT_USER_PW` is missing from the Profile, it gets a default value. Because default password is not an option, if you set an empty password explicitly in the Profile Cloudbreak deployer will ask for password all the time when it is needed for the operation.

    <pre>
export UAA_DEFAULT_USER_PW=''
</pre>

    In this case, Cloudbreak deployer wouldn't be able to add the default user, so you have to do it manually by executing the following command:

    <pre>
cbd util add-default-user
</pre>

For more information about setting environment variables in Profile, refer to [Configure Profile Variables](profile.md#configure-profile-variables).


### Adding SSL Certificate for Cloudbreak UI 

By default Cloudbreak has been configured with a self-signed certificate for access via HTTPS. This is sufficient for many deployments such as trials, development, testing, or staging. However, for production deployments, a trusted certificate is preferred and can be configured in the controller. Follow these steps to configure the cloud controller to use your own trusted certificate. 

**Prerequisites**

To use your own certificate, you must have:

* A resolvable fully qualified domain name (FQDN) for the controller host IP address. For example, this can be set up in [Amazon Route 53](https://aws.amazon.com/documentation/route53/).  
* A valid SSL certificate for this fully qualified domain name. The certificate can be obtained from a number of certificate providers.  

**Steps**

1. SSH to the Cloudbreak host instance:

    <pre>ssh -i mykeypair.pem cloudbreak@[CONTROLLER-IP-ADDRESS]</pre>
    
2. Make sure that the target fully qualified domain name (FQDN) which you plan to use for Cloudbreak is resolvable:

    <pre>nslookup [TARGET-CONTROLLER-FQDN]</pre>
    
    For example:

    <pre>nslookup hdcloud.example.com</pre>
    
3. Browse to the Cloudbreak deployment directory and edit the `Profile` file:

    <pre>vi /var/lib/cloudbreak-deployment/Profile</pre>
    
4. Replace the value of the `PUBLIC_IP` variable with the `TARGET-CONTROLLER-FQDN` value:

    <pre>PUBLIC_IP=[TARGET-CONTROLLER-FQDN]</pre>
    
5. Copy your private key and certificate files for the FQDN onto the Cloudbreak host. These files must be placed under `/var/lib/cloudbreak-deployment/certs/traefik/` directory.

    > File permissions for the private key and certificate files can be set to 600.

    | File | Example |
|---|---|
| PRIV-KEY-LOCATION	| /var/lib/cloudbreak-deployment/certs/traefik/hdcloud.example.com.key |
| CERT-LOCATION	| /var/lib/cloudbreak-deployment/certs/traefik/hdcloud.example.com.crt |

6. Configure TLS details in your `Profile` by adding the following line at the end of the file.

    > Notice that `CERT-LOCATION` and `PRIV-KEY-LOCATION` are file locations from Step 5, starting at the `/certs/...` path.

    <pre>export CBD_TRAEFIK_TLS=”[CERT-LOCATION],[PRIV-KEY-LOCATION]”</pre>
    
    For example:

    <pre>export CBD_TRAEFIK_TLS="/certs/traefik/hdcloud.example.com.crt,/certs/traefik/hdcloud.example.com.key"</pre>
    
7. Restart Cloudbreak deployer:

    <pre>cbd restart</pre>
    
8. Using your web browser, access to the Cloudbreak UI using the new resolvable fully qualified domain name.

9. Confirm that the connection is SSL-protected and that the certificate used is the certificate that you provided to Cloudbreak.


### Configure Access from Custom Domains

Cloudbreak Deployer, which uses UAA as an identity provider, supports multitenancy. In UAA, multitenancy is managed through identity zones. An identity zone is accessed through a unique subdomain. For example, if the standard UAA responds to `https://uaa.10.244.0.34.xip.io`, a zone on this UAA can be accessed through a unique subdomain `https://testzone1.uaa.10.244.0.34.xip.io`.

If you want to use a custom domain for your identity or deployment, add the `UAA_ZONE_DOMAIN` line to your `Profile`:

<pre>export UAA_ZONE_DOMAIN=my-subdomain.example.com</pre>

This variable is necessary for UAA to identify which zone provider should handle the requests that arrive to that domain.
