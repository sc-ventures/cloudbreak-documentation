## Add SSL certificate for Cloudbreak web UI 

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


