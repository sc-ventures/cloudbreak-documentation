## Upgrade Cloudbreak

To upgrade Cloudbreak to the newest version, perform the following steps.

We recommend that you back up Cloudbreak databases before upgrading. Refer to [Back up Cloudbreak Database](cb-migrate.md#back-up-cloudbreak-database).

**Steps**


1. On the VM where Cloudbreak is running, navigate to the directory where your Profile file is located:

    <pre>cd /var/lib/cloudbreak-deployment/</pre>

2. Stop all of the running Cloudbreak components:

    <pre>cbd kill</pre>
    
3. Update Cloudbreak deployer:

    <pre>cbd update</pre>
    
3. Update the `docker-compose.yml` file with new Docker containers needed for the cbd:

    <pre>cbd regenerate</pre>
    
4. If there are no other Cloudbreak instances that still use old Cloudbreak versions, remove the obsolete containers:

    <pre>cbd util cleanup</pre>
    
5. Check the health and version of the updated cbd:

    <pre>cbd doctor</pre>
    
6. Start the new version of the cbd:

    <pre>cbd start</pre>
    
    Cloudbreak needs to download updated docker images for the new version, so this step may take a while.

    
