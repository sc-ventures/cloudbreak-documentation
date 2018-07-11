## Upgrade Cloudbreak

To upgrade Cloudbreak to the newest version, perform the following steps. 

You must back up the Cloudbreak databases before upgrading. Refer to [Back up Cloudbreak Database](cb-migrate.md#back-up-cloudbreak-database).

> Do not use 'cbd update' to update to Cloudbreak 2.4.3. 

**Steps**

1. On the VM where Cloudbreak is running, navigate to the directory where your Profile file is located. For example: 

    <pre>cd /var/lib/cloudbreak-deployment/</pre>

2. Run the following commands to download the binary:    

    <pre>export CBD_VERSION=2.4.3  
curl -Ls public-repo-1.hortonworks.com/HDP/cloudbreak/cloudbreak-deployer_${CBD_VERSION}_$(uname)_x86_64.tgz | tar -xz -C /bin cbd</pre>

3. Verify the version:

    <pre>cbd version</pre>

4. Restart Cloudbreak:

    <pre>cbd restart</pre>
    
    