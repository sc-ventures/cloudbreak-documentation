
## Configure sccess from custom domains

Cloudbreak deployer, which uses UAA as an identity provider, supports multitenancy. In UAA, multitenancy is managed through identity zones. An identity zone is accessed through a unique subdomain. For example, if the standard UAA responds to `https://uaa.10.244.0.34.xip.io`, a zone on this UAA can be accessed through a unique subdomain `https://testzone1.uaa.10.244.0.34.xip.io`.

If you want to use a custom domain for your identity or deployment, add the `UAA_ZONE_DOMAIN` line to your `Profile`:

<pre>export UAA_ZONE_DOMAIN=my-subdomain.example.com</pre>

This variable is necessary for UAA to identify which zone provider should handle the requests that arrive to that domain.


