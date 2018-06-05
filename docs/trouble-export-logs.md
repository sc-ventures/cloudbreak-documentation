## Export logs for support case troubleshooting 

Cloudbreak includes the `cbd export-logs` command that allows you to export Cloudbreak logs into a bundle. When filing a support case ticket, you should attach the bundle to the support case ticket. 

**Steps** 
 
1. Access the Cloudbreak VM via SSH.

2. Navigate to your Cloudbreak deployment directory, typically `/var/lib/cloudbreak-deployment/`: 

    <pre>cd /var/lib/cloudbreak-deployment/</pre> 

3. Run the following command:  

    <pre>cbd export-logs</pre>
    
    This generates a file called cbd_export_timestamp.tar.gz.   
    Optionally you can customize the file name by specifying its name:  
    
    <pre>cbd export-logs my-bundle-name</pre> 

    The command collects all cloubdreak logs, configurations, firewall and ip tables configurations, os type and open ports. After collection, all data is automatically anonymized and compressed into a tar.gz archive. During the process temporary folders zre created and then removed. After the process finished, user can find the archive at the location where they issued the command.
    
4. Copy the bundle onto your machine, for example by using scp.   

5. When filing a support case ticket, attach the bundle to the  ticket.


