
## Change default Cloudbreak ports  

By default, Cloudbreak uses ports 80 (HTTP) and 443 (HTTPS) for accessing the Cloudbreak web UI. To change these port numbers, you must edit the Profile file.

Cloudbreak should not be running when you change the port numbers. Edit Profile either before you start Cloudbreak the first time or stop Cloudbreak before editing the file.

**Steps**  

1. Navigate to the Cloudbreak deployment directory (typically `/var/lib/cloudbreak-deployment`) and open the Profile file with a text editor. 

2. Add one or both of the following parameters, setting them to the port numbers that you want to use:

    <pre>export TRAEFIK_PORT_HTTP=111
export TRAEFIK_PORT_HTTPS=222</pre>

3. Start or restart Cloudbreak by using `cbd start` or `cbd restart`. 

4. This change affects Cloudbreak CLI configuration. When [configuring the CLI](cli-install.md#configure-the-cli), you must provide these ports as part of the server URL. For example: 

    <pre>cb configure --server http://cb.server.address:111 --username  test@hortonworks.com
cb configure --server https://cb.server.address:222 --username  test@hortonworks.com</pre>
